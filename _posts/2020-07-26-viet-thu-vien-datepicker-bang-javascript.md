---
layout: post
title: 'Viết component datepicker bằng javascript thuần'
description: 'làm việc với các framework frontend như Vue, Angular hay React thì sẽ không ít lần dùng các thư viện datepicker, bài này sẽ hướng dẫn bạn tạo component datepicker bằng javascript thuần'
date: 2020-07-26
author: Giangg
color: rgb(255,210,32)
cover: '/assets/images/nes-datepicker.png'
tags: ReactJS Datepicker Javascript
---

Khi các bạn làm việc với các framework frontend như Vue, Angular hay React (thực ra nó là thư viện, nhưng mình cứ coi như nó là framework đi), bạn sẽ không ít lần dùng các thư viện datepicker có ở trên github. Đã bao giờ bạn tự hỏi rằng những thư viện này được viết như thế nào chưa? Lúc đầu mình cũng nghĩ những thư viện này ẩn chứa 1 thuật toán rất chi là cao siêu, nhưng khi bắt đầu viết cho mình 1 component datepicker thì lại thấy nó cực kỳ đơn giản. Hãy cùng bắt đầu nhé

## Thành quả

Đây là kết quả cuối cùng của component. Mình có sử dụng thư viện NES css để trông cho đẹp mắt hơn. demo thì ở đây [nes datepicker](https://gianglevan94.github.io/ugly-date-picker)

![datepicker](/assets/images/nes-datepicker.png)

## Thuật toán 

Mô ta 1 chút về component datepicker này. Như các bạn thấy nó sẽ gồm 35 ô số có thể chứa ngày của tháng hiện tại, ngày của tháng trước, và ngày của tháng kế tiếp. Ngày đầu tiên của tháng hiện tại sẽ luôn nằm ở hàng đầu tiên, vậy mấu chốt ở đây là bạn phải xác định được ngày đầu tiên của tháng là vào thứ mấy, rồi từ đấy fill vào các ô trước đó là ngày của tháng trước. Dưới đây là một vài đoạn code của thuật thoán. 

Đầu tiên là init các ngày của mảng mình sẽ có hàm `getDays` bên trong đấy sẽ có 2 hàm là `getDaysOfPrevMonth` và `getDaysOfMonth` sẽ trả về các ngày của tháng trước, và các ngày sau đó. Vì sao đoạn này mình lại `days.slice(0, MAX_DAYS)` ? Đoạn này do mình lười tính toàn số ngày của tháng hiện tại và tháng tiếp đó nên mình sẽ lấy dư ra vài ngày và cắt bớt. Bạn đọc code sẽ hiểu thêm.

```javascript
const getDays = (month) => {
  const days = [...getDaysOfPrevMonth(month), ...getDaysOfMonth(month)];

  return days.slice(0, MAX_DAYS);
};
```

Đây là hàm lấy gày của tháng hiện tại như bạn có thể thấy là hàm `getFirstDayIndex` sẽ trả về ngày đầu tiên của tháng là index thứ bao nhiêu của mảng. Sau đó sẽ lùi dần về ngày của các tháng trước đó

```js
const getDaysOfPrevMonth = (month) => {
  const firstDayIndex = getFirstDayIndex(month);
  const days = [];
  for (let i = 0; i < firstDayIndex; i++) {
    const now = new Date();
    now.setMonth(month);
    now.setDate(1);
    const diffDate = firstDayIndex - i;
    now.setDate(now.getDate() - diffDate);
    days.push(now);
  }

  return days;
};
```

Mình sẽ dùng hàm `getDay` để lấy ra thứ hiện tại trong tháng (thứ 2, chủ nhật...) đồng thời cũng là index của mảng luôn. Ngoài ra javascript còn có hàm `getDate` trả về ngày hôm đấy là ngày bao nhiêu của tháng nữa. 2 hàm này rất dễ lẫn lộn với nhau.

```js
const getFirstDayIndex = (month) => {
  const now = new Date();
  now.setMonth(month);
  now.setDate(1);

  return now.getDay();
};
```

Tiếp đến sẽ là hàm lấy tất cả những ngày còn lại. `MAX_DAYS` của mình đang để là 35.

```js
const getDaysOfMonth = (month) => {
  const days = [];
  for (let i = 1; i <= MAX_DAYS; i++) {
    const now = new Date();
    now.setMonth(month);
    now.setDate(i);
    days.push(now);
  }

  return days;
};
```

Khá là đơn giản đúng không. Như vừa rồi mình đã generate ra được 1 mảng 35 phần tử có chứa các ngày trong tháng để hiện lên calendar rồi. Việc còn lại chỉ là loop rồi hiển thị ra màn hình, đoạn này mình sẽ không đi chi tiết do bài này mình chỉ muốn giải thích về thuật toán để render ra cái calendar thôi. Khi đã render ra calendar thì mình nghĩ đã xong đến 70% component rồi. Ngoài ra sẽ còn những thứ râu ria như là: nút chuyển tháng, input, style cho đẹp..., cái này mình không đề cập ở đây. Bạn có thể xem source code hoàn chỉnh ở đây nhé [https://github.com/gianglevan94/ugly-date-picker](https://github.com/gianglevan94/ugly-date-picker)

## Lời kết
Vừa rồi là một bài hướng dẫn nho nhỏ về viết component datepicker bằng javascript thuần. Hi vọng bài viết sẽ hữu ích cho các bạn. Các bạn có thể tự build 1 component datepicker cho riêng mình (không khuyến khích), thay vì phải dùng những thư viện trên mạng có nhiều tính năng thừa thãi mà bạn không dùng đến cũng như sẽ dễ dàng tùy biến hơn tùy vào dự án và yêu cầu của khác hàng. Cám ơn các bạn đã đọc bài!


