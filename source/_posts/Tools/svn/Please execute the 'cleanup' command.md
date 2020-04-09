---
title: Previous operation has not finished; run 'cleanup' if it was interrupted
date: 2020-3-2 10:20:20
tags: [svn, tools]
categories: Tools
---

### 正文

---
使用svn提交代码时，显示无可用代码提交，当我更新代码时，svn报错：

```
Error   Previous operation has not finished; run 'cleanup' if it was interrupted
Error   Please execute the 'Cleanup' command
```
这是由于上一次更新或者提交代码时发生中断，导致上一次工作没有正常结束。

但是，当我右键->TortoiseSVN->clean up时，却报错：
```
Project E:\xxx\xxx\xxx is already locked
```
于是我又右键->unlock,结果还是提示：
`Previous operation has not finished; run 'cleanup' if it was interrupted`
这表示，有比当前目录更高级的目录发生了异常，当前目录处理locded状态，于是我返回项目根目录：右键->TortioseSVN->clean up
这次终于正常执行了。
clean up完成后，目录上也显示出了绿勾和红叹号
随后提交代码也恢复正常

### 总结

---
- svn无法提交或更新，大多是由上一次命令异常中断引起的（可能性有很多，idea中断等某些原因）。
- 清理缓存clean up
- 无法clean up有可能是由于当前目录处于锁定状态，是因为他的上级目录锁定
- 给上级目录clean up,或更上级（可根目录，即.svn所在的目录）
- 注意提前备份代码