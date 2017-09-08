Yii2.0.7 扩展-百度编辑器（Ueditor）（非composer安装图片本地上传）

Yii：2.0.6

Ueditor：1.4.3.1 （php版本）

来源：https://github.com/org-yii-china/yii2-ueditor

安装方法：

1.下载yii2-ueditor
（pscheckout、删除git信息，纳入版本库，）

2.将文件方在 根目录/common/widgets 下即可

调用方法：

在rootPath/backend/controllers中新建一个控制器加入以下代码

    public function actions(){
        return [
            'ueditor'=>[
                'class' => 'common\widgets\ueditor\UeditorAction',
                /*'config'=>[
                    //上传图片配置
                    'imageUrlPrefix' => "", /* 图片访问路径前缀 */
                    'imagePathFormat' => "/image/{yyyy}{mm}{dd}/{time}{rand:6}", /* 上传保存路径,可以自定义保存路径和文件名格式 */
                ]*/
            ]
        ];
    }
    //排除权限控制
    public function allowActions(){
        return ['ueditor'];
    }

第一种调用方式：

在对应的渲染页面，即views下的页面中

```php
<?= common\widgets\ueditor\Ueditor::widget(['options'=>['initialFrameWidth' => 850,]])?>
```

options 填写配置编辑器的参数（参考ueditor官网）或者 \common\widgets\ueditor\config.php

第二种调用方式：

    <?php $form = ActiveForm::begin(); ?>

    <?= $form->field($model, 'title')->textInput(['maxlength' => true]) ?>

    <?= $form->field($model,'content')->widget('common\widgets\ueditor\Ueditor',['options'=>['maximumWords'=>10000,'initialFrameWidth'=>1200,'initialFrameHeight'=>300]]); ?>

    
      ...
      
    <?php ActiveForm::end(); ?>
    
    
 三、遇到的问题：（http://www.blogxuan.com/php/show/323.html）
UEditor 编辑器点击上传文件选择框会延迟几秒才会显示，反应很慢，上传图片选择框显示很慢。

1、common\widgets\ueditor\vendor\dialogs\image\image.js中的下面代码

accept: {
  title: 'Images',
  extensions: 'jpg,jpeg,png',
  mimeTypes: 'image/*'}

修改为：

accept: {
  title: 'Images',
  extensions: 'jpg,jpeg,png',
  mimeTypes: 'image/jpg,image/jpeg,image/png,image/gif' //修改这行  
}

2、common\widgets\ueditor\vendor\ueditor.all.js和ueditor.all.min.js中，搜索accept，将其后面的

accept="image/*"

修改为：

accept="image/jpg,image/jpeg,image/png,image/gif"
