### svn 命令行

```
svn st | awk '{if ( $1 == "?") { print $2}}' | xargs svn add

svn st | awk '{if ( $1 == "!") { print $2}}' | xargs svn delete

find . -type d -name ".svn"|xargs rm -rf

svn ci -m ''

// 更新到指定版本
svn log 
svn up -r r339609

```

