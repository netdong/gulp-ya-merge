# gulp-ya-merge

用来合并html页面里边的js和css，并对js和css增加文件版本号。

## 用法
```js
var destDir = "dist";
var merge = require('gulp-ya-merge');

gulp.task('merge', function () {
  gulp.src('mall/**/*.phtml')
    .pipe(merge({
      rootPath: process.cwd() + '/share/static/',
    }))
    .pipe(gulp.dest('/dest/mall/'));
});
```

## 定制
目前没有提供选项修改配置项，如果需要修改，请找到index.js文件，修改下面的代码即可：
```js
const matchFlags = {
  // 用来匹配合并区间内的文件地址
  'fileExp': /<\?\=\s*\$this\->StaticUrl\(\s*\'([^\']+)\'\s*\)\s*\?>/gm,
  // 合并区间代码的起始标识
  'leftFlag': '<!--min[',
  // 合并区间代码的结束标识
  'rightFlag': ']-->',
  // 合并后要替换的地址。{$base}代表目录及文件名（不包括后缀），{$stamp}代表文件版本号，{$ext}代表文件的后缀
  'newPath': '<?=$this->StaticUrl(\'{$base}-{$stamp}{$ext}\')?>',
  // 文件版本号的长度
  'hashLength': 8,
  // 要替换的非合并的js的正则表达式
  'jsExp': /(<script\s(?:.*)\$this->StaticUrl\([\'\"])([^\'\"\-]+)([\'\"]\)(?:[^\/]*)><\/script>)/gm,
  // 要替换的非合并的css的正则表达式
  'cssExp': /(<link\s(?:.*)\$this->StaticUrl\([\'\"])([^\'\"\-]+)([\'\"]\)(?:[^\/]*)(?:\/?>|<\/link>))/gm
};
```


比如：
```php
<link rel="shortcut icon" type="image/x-icon" href="<?= $this->StaticUrl('/static/favicon.ico') ?>"/>
<?php if (isset($this->v['mobileUrl'])): ?>
    <meta name="mobile-agent" content="format=html5; url=<?= $this->v['mobileUrl'] ?>"/>
<?php endif ?>
<?php if (isset($this->v['is_seo_tag']) && !empty($this->v['is_seo_tag'])): ?>
    <!-- <meta name="robots" content="nofollow">-->
<?php endif; ?>
<!--min[style /static/css/jiehun.n.min.css]-->
<link href="<?= $this->StaticUrl('/static/css/reset.css') ?>" rel="stylesheet" type="text/css"/>
<link href="<?= $this->StaticUrl('/static/css/grid.css') ?>" rel="stylesheet" type="text/css"/>
<link href="<?= $this->StaticUrl('/static/css/common.css') ?>" rel="stylesheet" type="text/css"/>
<link href="<?= $this->StaticUrl('/static/css/layout.css') ?>" rel="stylesheet" type="text/css"/>
<link href="<?= $this->StaticUrl('/static/css/class.css') ?>" rel="stylesheet" type="text/css"/>
<!--min[style]-->
<!--min[hapj /static/js/hapj.n.min.js]-->
<script src="<?= $this->StaticUrl('/static/js/jquery.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/hapj.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/switchable.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/verifiable.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/floatable.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/menuable.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/ajaxable.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/dialog.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/selectable.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/cal.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/lazyload.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/ui/sortable.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/mod/adorn.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/hapj.conf.js') ?>"></script>
<script src="<?= $this->StaticUrl('/static/js/hapj.hook.js') ?>"></script>
<!--min[hapj]-->
```

经过`gulp-ya-merge`的处理，最后变成了如下代码：
  
```php
<link rel="shortcut icon" type="image/x-icon" href="<?= $this->StaticUrl('/static/favicon-ba5c0613.ico') ?>"/>
<?php if (isset($this->v['mobileUrl'])): ?>
    <meta name="mobile-agent" content="format=html5; url=<?= $this->v['mobileUrl'] ?>"/>
<?php endif ?>
<?php if (isset($this->v['is_seo_tag']) && !empty($this->v['is_seo_tag'])): ?>
    <!-- <meta name="robots" content="nofollow">-->
<?php endif; ?>
<link rel="stylesheet" type="text/css" href="<?=$this->StaticUrl('/static/css/jiehun.n.min-d41d8cd9.css')?>"/>
<script src="<?=$this->StaticUrl('/static/js/hapj.n.min-d41d8cd9.js')?>"></script>
```
