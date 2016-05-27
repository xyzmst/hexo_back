title: git常见问题处理
date: 2016-05-27 15:51:13
tags:
---
- 1.You have not concluded your merge (MERGE_HEAD exists)
场景：无法pull或push
处理方法：还原merge
git merge --abort 
git reset --merge 
然后重新pull 处理 merge
