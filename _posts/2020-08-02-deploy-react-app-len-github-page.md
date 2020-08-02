---
layout: post
title: 'Deploy react app lên github page'
description: 'nếu bạn muốn gửi link cho bạn bè để khoe thành quả, hay deploy để test thì bạn làm như thế nào? github page là một nơi tốt để deploy React app'
date: 2020-08-02
author: Giangg
color: rgb(255,210,32)
cover: '/assets/images/react-app.png'
tags: ReactJS S3 Amazon
---

Khi phát triển một trang sử dụng ReactJS, nếu bạn muốn gửi link cho bạn bè để khoe thành quả, hay deploy để test thì bạn làm như thế nào? Amazon s3 là một nơi tốt để deploy React app. Và tuyệt vời hơn nữa là nó hoàn toàn miễn phí.

## Nội dung chính
- Tạo 1 react app đơn giản
- Cấu hình để deploy lên github page

## Cần chuẩn bị
- Tài khoản github

### Tạo react app đơn giản
Mình sẽ dùng `create-react-app` để tạo 1 react app để demo. 
```
create-react-app demo-github-pages
cd demo-github-pages
npm start
```
Và đây là thành quả

![react app](/assets/images/react-app.png)

## Cấu hình để deploy

Sau đó thì cài đặt thư viện `gh-pages` bằng cách chạy
```
npm install gh-pages --save-dev
```

Tiếp đến là thay đổi một chút thông tin của file `package.json`. Đầu tiên bạn cần làm là thêm địa chỉ homepage của app. url sẽ có dạng https://<username github>.github.io/<tên của repo>. Ví dụ như dưới là username github của mình là `gianglevan94` còn tên của repo là `ugly-date-picker`.

```
"name": "date-picker",
"version": "0.1.0",
"private": true,
"homepage": "https://gianglevan94.github.io/ugly-date-picker",
"dependencies": {
    "@testing-library/jest-dom": "^4.2.4",
 ///   
```

Sau đó thì cập nhật lại scripts 1 tí

```
"scripts": {
    "start": "react-scripts start",
    "deploy": "gh-pages -d build",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
},
```
![package json](/assets/images/gh-page-package-json.png)

Bước tiếp theo là bạn tạo 1 repo trên github. Nhớ tạo đúng tên như mục `homepage` ở trên. 

## Deploy
- Đầu tiên là chạy lệnh build `npm run build`
- Tiếp theo là chạy lệnh deploy `npm run deploy`

Sau khi hoàn chạy hoàn tất script thì bạn vào địa chỉ `homepage` vừa cấu hình lúc nãy để xem thành quả. Nhớ là clear cache trước nhé.

Trên đây là một bài hướng dẫn nho nhỏ để deploy react app lên github pages. Hi vọng sẽ hữu ích cho bạn. Nếu có bất cứ thắc mắc gì thì hãy comment bên dưới nhé. Cảm ơn bạn đã đọc bài.




