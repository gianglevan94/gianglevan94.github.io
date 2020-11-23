---
layout: post
title: 'Xóa sự kiện beforeunload trong javascript'
description: 'Trong file script có sẵn của kintone thì đã implement sẵn sự kiện onbeforeunload để thông báo khi rời trang. Nhiều khi cái thông báo đó rất là khó chịu. Và hôm nay mình đã phải bỏ sự kiện onbeforeunload trong một file script khác.'
date: 2020-11-23
author: Giangg
color: rgb(255,210,32)
cover: '/assets/images/before-unload.png'
tags: Kintone Javascript
---

Ngày hôm nay mình đã gặp được 1 vấn đề khá là hay ho, nên khi về đến nhà, mình đã quyết định viết bài này để giới thiệu với mọi người và cũng coi như là bookmark lại cho tương lai lỡ có nếu gặp phải thì chỉ cần vào đây để xem lại. 

## Vấn đề

Vấn đề mình gặp phải là: team mình đang phải làm việc với `kintone`, một công nghệ khá là củ củ chuối. Đại loại là kintone sẽ cung cấp giao diện kéo thả, dev sẽ viết code javascript để nhúng vào custom giao diện, cũng như là tính năng. Trong file script có sẵn của `kintone` thì đã implement sẵn sự kiện `onbeforeunload` để thông báo khi rời trang. Nhiều khi cái thông báo đó rất là khó chịu. Và hôm nay mình đã phải bỏ sự kiện `onbeforeunload` trong một file script khác.

![beforeunload](/assets/images/before-unload.png)

## Giải pháp

Đầu tiên là mình search trên google, cũng ra khá nhiều cách nhưng mình thử đều không được. Như là `window.onbeforeunload = null`. Cũng đã thử mò vào source code của kintone để xem nhưng file đó đã được build rồi, nên là không thể đọc được nó viết gì. 

Sau đó thì mình nảy ra ý tưởng là sẽ ghi đè hàm `window.addEventListener` khi mà listener `beforeunload` thì mình sẽ không làm gì, đại loại là không cho listener sự kiện beforeunload. Code version 1 sẽ như sau

```javascript
export const disableBeforeUnload = () => {
    const oldAddEventListener = window.addEventListener;
    window.addEventListener = (...args) => {
        const [type, listener] = args;
        if(type === 'breforeunload') {
            return
        }
        oldAddEventListener(...args);
    }
}

```

Bạn chỉ cần gọi hàm này ở đầu file javascript thì mọi sự kiện `beforeunload` sẽ không được thực thi.

Hãy cùng nâng cấp chức năng của hàm này lên 1 chút nhé. Mình muốn bình thường vẫn listener sự kiện `beforeunload` khi nào mình gọi hàm này thì mới bỏ sự kiện đó đi. Và khi nào mình muốn listener lại cũng được. Kiểu như tạm dừng không cho gọi `beforeunload`. Code xử lý sẽ như sau

```javascript
const listeners = [];
const oldAddEventListener = window.addEventListener;
window.addEventListener = (...args) => {
    const [type, listener] = args;
    if(type === 'breforeunload') {
        listeners.push(listener)
    }
    oldAddEventListener(...args);
}

export const disableBeforeUnload = () => {
    listeners.forEach(listener => {
        window.removeEventListener('beforeunload', listener);
    })
}

export const enableBeforeUnload = () => {
    listeners.forEach(listener => {
        window.addEventListener('beforeunload', listener);
    })
}

```

Khá là đơn giản đúng không. Hi vọng bài viết này sẽ hữu ích với các bạn. 

