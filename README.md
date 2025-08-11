# olist-onlyoffice-preview
为Alist/Openlist提供OnlyOffice预览支持

# 使用方法
1. 将view.html拷贝到Nginx能访问的位置
2. 部署Onlyoffice Jwt密钥认证后端
   参照 [https://github.com/zhaxingyu/ds-token-gen](https://github.com/zhaxingyu/ds-token-gen)
3. 配置反向代理，添加以下两个Location
```conf
  location = /view.html {
    root /config/onlyoffice;#将此处改为你的view.html所在的位置
    try_files $uri =404;
  }
  
  location /generate-token {
    # 假设你的 Node.js 服务运行在 192.168.31.200 的 3000 端口
    proxy_pass http://192.168.31.200:3000;
  }
```
4. 在Alist/OpenList的设置-预览-iFrame预览中更改为以下字段
```json
{
  "doc,docx,xls,xlsx,ppt,pptx,odt,ods,odp,rtf,txt,csv": {
    "OnlyOffice": "https://<你的OnlyOffice服务器地址>/view.html?src=$e_url",
    "Google": "https://docs.google.com/gview?url=$e_url&embedded=true"
  },
  "pdf": {
    "PDF.js": "https://alist-org.github.io/pdf.js/web/viewer.html?file=$e_url",
    "OnlyOffice": "https://<你的OnlyOffice服务器地址>/view.html?src=$e_url"
  },
  "epub": {
    "EPUB.js": "https://alist-org.github.io/static/epub.js/viewer.html?url=$e_url",
    "OnlyOffice": "https://<你的OnlyOffice服务器地址>/view.html?src=$e_url"
  }
}
```
