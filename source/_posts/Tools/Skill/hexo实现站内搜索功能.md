---
title: hexo博客实现站内文章搜索功能
date: 2020-3-30
tags: [skill, tools]
categories: Tools
---

### 正文

---
这里首先介绍一个插件**hexo-generator-search**一个专为hexo实现站内搜索打造的工具，话说社区的大佬们是真的无敌……
咳咳，其实我也不知道他是不是专为hexo而生的，但我知道他确实在这一方面实现了无比价值。

这个插件是写的真好啊，搜索功能实在强大，不止能搜索标题，还能全文检索！！速度还快的不行。此处对应的js也是在巨人的肩膀上。

### 安装插件

---
在博客根目录下执行：
````
npm install --save hexo-generator-search
````

### 生成搜索框

---
- 加到对应的index.ejs里面去

先写个搜索框出来
````
<a href="javascript:;" class="popup-trigger"><i class="menu-item-icon fa fa-search fa-fw"></i></a>
<div class="popup search-popup local-search-popup">
  <div class="local-search-header clearfix">
    <span class="search-icon">
      <i class="fa fa-search"></i>
    </span>
    <span class="popup-btn-close">
      <i class="fa fa-times-circle"></i>
    </span>
    <div class="local-search-input-wrapper">
      <input autocomplete="off"
             placeholder="<%- theme.local_search.placeholder%>" spellcheck="false"
             type="text" id="local-search-input">
    </div>
  </div>
  <div id="local-search-result"></div>
</div>
````
然后在.ejs中加载搜索功能的js(这里可以根据自己的喜好设计样式)

### js解析函数

---
````
/* global CONFIG */

$(document).on("DOMContentLoaded", function() {
	debugger;
  // Popup Window
  var isfetched = false;
  var datas;
  var isXml = true;
  // Search DB path
  var searchPath = CONFIG.path;
  if (searchPath.length === 0) {
    searchPath = "search.xml";
  } else if (/json$/i.test(searchPath)) {
    isXml = false;
  }
  var path = CONFIG.root + searchPath;
  var input = document.getElementById("local-search-input");
  var resultContent = document.getElementById("local-search-result");

  // Ref: https://github.com/ForbesLindesay/unescape-html
  function unescapeHtml(html) {
    return String(html)
      .replace(/&#39;/g, "\'")
      .replace(/&#x3A;/g, ":")
      // Replace all the other &#x; chars
      .replace(/&#(\d+);/g, function(m, p) {
        return String.fromCharCode(p);
      })
      .replace(/&lt;/g, "<")
      .replace(/&gt;/g, ">")
      .replace(/&amp;/g, "&");
  }
  function getIndexByWord(word, text, caseSensitive) {
    var wordLen = word.length;
    if (wordLen === 0) {
      return [];
    }
    var startPosition = 0; var position = []; var index = [];
    if (!caseSensitive) {
      text = text.toLowerCase();
      word = word.toLowerCase();
    }
    while ((position = text.indexOf(word, startPosition)) > -1) {
      index.push({
        position,
        word
      });
      startPosition = position + wordLen;
    }
    return index;
  }

  // Merge hits into slices
  function mergeIntoSlice(text, start, end, index, searchText) {
    var item = index[index.length - 1];
    var position = item.position;
    var word = item.word;
    var hits = [];
    var searchTextCountInSlice = 0;
    while (position + word.length <= end && index.length !== 0) {
      if (word === searchText) {
        searchTextCountInSlice++;
      }
      hits.push({
        position,
        length : word.length
      });
      var wordEnd = position + word.length;

      // Move to next position of hit
      index.pop();
      while (index.length !== 0) {
        item = index[index.length - 1];
        position = item.position;
        word = item.word;
        if (wordEnd > position) {
          index.pop();
        } else {
          break;
        }
      }
    }
    return {
      hits,
      start,
      end,
      searchTextCount : searchTextCountInSlice
    };
  }

  // Highlight title and content
  function highlightKeyword(text, slice) {
    var result = "";
    var prevEnd = slice.start;
    slice.hits.forEach(function(hit) {
      result += text.substring(prevEnd, hit.position);
      var end = hit.position + hit.length;
      result += `<b class="search-keyword">${text.substring(hit.position, end)}</b>`;
      prevEnd = end;
    });
    result += text.substring(prevEnd, slice.end);
    return result;
  }
  function inputEventFunction() {
    var searchText = input.value.trim().toLowerCase();
    var keywords = searchText.split(/[-\s]+/);
    if (keywords.length > 1) {
      keywords.push(searchText);
    }
    var resultItems = [];
    if (searchText.length > 0) {
      // Perform local searching
      datas.forEach(function(data) {
        // Only match articles with not empty titles
        if (!data.title) {
          return;
        }
        var searchTextCount = 0;
        var title = data.title.trim();
        var titleInLowerCase = title.toLowerCase();
        var content = data.content ? data.content.trim().replace(/<[^>]+>/g, "") : "";
        if (CONFIG.localsearch.unescape) {
          content = unescapeHtml(content);
        }
        var contentInLowerCase = content.toLowerCase();
        var articleUrl = decodeURIComponent(data.url).replace(/\/{2,}/g, "/");
        var indexOfTitle = [];
        var indexOfContent = [];
        keywords.forEach(function(keyword) {
          indexOfTitle = indexOfTitle.concat(getIndexByWord(keyword, titleInLowerCase, false));
          indexOfContent = indexOfContent.concat(getIndexByWord(keyword, contentInLowerCase, false));
        });

        // Show search results
        if (indexOfTitle.length > 0 || indexOfContent.length > 0) {
          var hitCount = indexOfTitle.length + indexOfContent.length;
          // Sort index by position of keyword
          [indexOfTitle, indexOfContent].forEach(function(index) {
            index.sort(function(itemLeft, itemRight) {
              if (itemRight.position !== itemLeft.position) {
                return itemRight.position - itemLeft.position;
              }
              return itemLeft.word.length - itemRight.word.length;
            });
          });

          var slicesOfTitle = [];
          if (indexOfTitle.length !== 0) {
            var tmp = mergeIntoSlice(title, 0, title.length, indexOfTitle, searchText);
            searchTextCount += tmp.searchTextCountInSlice;
            slicesOfTitle.push(tmp);
          }

          var slicesOfContent = [];
          while (indexOfContent.length !== 0) {
            var item = indexOfContent[indexOfContent.length - 1];
            var position = item.position;
            var word = item.word;
            // Cut out 100 characters
            var start = position - 20;
            var end = position + 80;
            if (start < 0) {
              start = 0;
            }
            if (end < position + word.length) {
              end = position + word.length;
            }
            if (end > content.length) {
              end = content.length;
            }
            let tmp = mergeIntoSlice(content, start, end, indexOfContent, searchText);
            searchTextCount += tmp.searchTextCountInSlice;
            slicesOfContent.push(tmp);
          }

          // Sort slices in content by search text's count and hits' count
          slicesOfContent.sort(function(sliceLeft, sliceRight) {
            if (sliceLeft.searchTextCount !== sliceRight.searchTextCount) {
              return sliceRight.searchTextCount - sliceLeft.searchTextCount;
            } else if (sliceLeft.hits.length !== sliceRight.hits.length) {
              return sliceRight.hits.length - sliceLeft.hits.length;
            }
            return sliceLeft.start - sliceRight.start;
          });

          // Select top N slices in content
          var upperBound = parseInt(CONFIG.localsearch.top_n_per_article, 10);
          if (upperBound >= 0) {
            slicesOfContent = slicesOfContent.slice(0, upperBound);
          }

          var resultItem = "";

          if (slicesOfTitle.length !== 0) {
            resultItem += `<li><a href="${articleUrl}" class="search-result-title">${highlightKeyword(title, slicesOfTitle[0])}</a>`;
          } else {
            resultItem += `<li><a href="${articleUrl}" class="search-result-title">${title}</a>`;
          }

          slicesOfContent.forEach(function(slice) {
            resultItem += `<a href="${articleUrl}"><p class="search-result">${highlightKeyword(content, slice)}...</p></a>`;
          });

          resultItem += "</li>";
          resultItems.push({
            item : resultItem,
            searchTextCount,
            hitCount,
            id : resultItems.length
          });
        }
      });
    }
    if (keywords.length === 1 && keywords[0] === "") {
      resultContent.innerHTML = '<div id="no-result"><i class="fa fa-search fa-5x"></i></div>';
    } else if (resultItems.length === 0) {
      resultContent.innerHTML = '<div id="no-result"><i class="fa fa-frown-o fa-5x"></i></div>"';
    } else {
      resultItems.sort(function(resultLeft, resultRight) {
        if (resultLeft.searchTextCount !== resultRight.searchTextCount) {
          return resultRight.searchTextCount - resultLeft.searchTextCount;
        } else if (resultLeft.hitCount !== resultRight.hitCount) {
          return resultRight.hitCount - resultLeft.hitCount;
        }
        return resultRight.id - resultLeft.id;
      });
      var searchResultList = '<ul class="search-result-list">';
      resultItems.forEach(function(result) {
        searchResultList += result.item;
      });
      searchResultList += "</ul>";
      resultContent.innerHTML = searchResultList;
    }
  }
  function fetchData(callback) {
    $.ajax({
      url     : path,
      dataType: isXml ? "xml" : "json",
      success (res) {
        // Get the contents from search data
        isfetched = true;
        datas = isXml ? $("entry", res).map(function() {
          return {
            title : $("title", this).text(),
            content : $("content", this).text(),
            url : $("url", this).text()
          };
        }).get() : res;

        // Remove loading animation
        $(".local-search-pop-overlay").remove();
        $("body").css("overflow", "");

        if (callback) {
          callback();
        }
      }
    });
  }
  if (CONFIG.localsearch.preload) {
    fetchData();
  }

  // Monitor main search box
  function onPopupClose() {
    $(".popup").hide();
    $("#local-search-input").val("");
    $(".search-result-list").remove();
    $("#no-result").remove();
    $(".local-search-pop-overlay").remove();
    $("body").css("overflow", "");
  }

  function proceedSearch() {
    $("body")
      .append('<div class="local-search-pop-overlay"></div>')
      .css("overflow", "hidden");
    $(".local-search-pop-overlay").click(onPopupClose);
    $(".popup").show();
    $("#local-search-input")
      .attr("autocapitalize", "none")
      .attr("autocorrect", "off")
      .focus();
  }

  // Search function
  function searchFunc() {
    // Start loading animation
    $("body")
      .append(`<div class="local-search-pop-overlay">
          <div id="search-loading-icon">
            <i class="fa fa-spinner fa-pulse fa-5x fa-fw"></i>
          </div>
        </div>`)
      .css("overflow", "hidden");
    $("#search-loading-icon").css({
      margin      : "20% auto 0 auto",
      "text-align": "center"
    });
    fetchData(proceedSearch);
  }

  if (CONFIG.localsearch.trigger === "auto") {
    input.addEventListener("input", inputEventFunction);
  } else {
    $(".search-icon").click(inputEventFunction);
    input.addEventListener("keypress", function(event) {
      if (event.keyCode === 13) {
        inputEventFunction();
      }
    });
  }

  // Handle and trigger popup window
  $(".popup-trigger").click(function(e) {
    e.stopPropagation();
    if (isfetched === false) {
      searchFunc();
    } else {
      proceedSearch();
    }
  });

  $(".popup-btn-close").click(onPopupClose);
  $(".popup").click(function(e) {
    e.stopPropagation();
  });
  $(document).on("keyup", function(event) {
    var shouldDismissSearchPopup = event.which === 27 && $(".search-popup").is(":visible");
    if (shouldDismissSearchPopup) {
      onPopupClose();
    }
  });
});
````
ps: 这个path其实就是安装插件后生成的search.xml,也可以直接指定此文件的完整路径(外网必须可以请求的到)

### 修改hexo配置文件

---
修改根目录下的`_config.yml`,直接在最底部添加：
````
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
````
ps: yml语言缩进敏感哟

### 修改主题配置文件

---
主题根目录下的`_config.yml`
````
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
````

### 结束

---
最后重启一下hexo服务就好啦，如果没有效果就clean一下。