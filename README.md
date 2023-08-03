# 开发文档

## 文档编写

修改docs/source/hundunapidocs.md文件

## 编译

```

cd apidocs/docs

# 会在docs/build/html目录下编译出web文件，可以打开index.html本地查看

# 需要安装依赖，请参照 https://docs.readthedocs.io/en/stable/tutorial/ 

make html 

```

## 查看生成网页

到docs/build/html查看index.html文件即可

## 上传github

上传后会自动触发build操作。
