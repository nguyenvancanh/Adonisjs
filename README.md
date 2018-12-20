# Adonisjs

Thời gian gần đây, NodeJs là ngôn ngữ lập trình đang được những nhà lập trình viên quan tâm nhiều nhất. Những dự án, sản phẩm sử dụng NodeJs ra đời ngày càng nhiều. Lập trình viên chuyển sang làm NodeJs ngày càng tăng. Không quá khi nói NodeJs là ngôn ngữ lập trình đang "HOT" nhất hiện nay. Hòa theo không khí chung của xu thế, là một lập trình viên PHP, tôi cũng thử đá sang học NodeJs để có thể sử dụng trong một tương lai không xa. Cũng như PHP, NodeJs cũng có những framework để hỗ trợ cho lập trình viên trong quá trình dev sản phẩm, phần mềm. Với những nhà lập trình PHP, chắc hẳn không ai là không biết tới framework Laravel, một framework sẽ là lựa chọn hàng đầu cho những nhà lập trình viên khi lựa chọn framework nào để xây dựng mới một ứng dụng web. Từ là một dev về PHP, lại mới học về NodeJs, tôi mong muốn có một framework nào gần gũi, quen thuộc dễ sử dụng với tôi nhất nhưng lại dùng để code NodeJs. Sau một thời gian lang thang trên mạng tìm kiếm, tôi tìm được framework **AdonisJs**, một MVC framework cho NodeJs có cấu trúc tương tự như Laravel, quá dễ học và dễ sử dụng cho tôi. Tôi tổng hợp lại những gì mình đọc, hiểu trong bài viết này cho các bạn có chung đam mê cùng tìm hiểu.

NodeJs phát triền không ngừng, các framework ra đời để phục vụ việc dev NodeJs cũng không hề ít, Vậy vì sao chúng ta nên lựa chọn **AdonisJs** làm framework khi bắt đầu dự án ngoài lý do nó có cấu trúc tương tự như Laravel?

- Github của **AdonisJs** đã có hơn 5200 sao. [https://github.com/adonisjs/adonis-framework](https://github.com/adonisjs/adonis-framework) 
- Pull request gần nhất cách đây chưa đến 2 tháng. 
- Đã có 821 Issue được Close.

=> Khi bạn sử dụng ***AdonisJs** thì có thể hoàn toàn yên tâm khi gặp bất cứ vấn đề  nào, đảm bảo các bạn sẽ được hỗ trợ nhanh chóng và chính xác.

## Cấu trúc thư mục

Vì nó là một MVC framework tương tự như Laravel, nên cấu trúc thư mục của **AdonisJs** cũng không khác:

```
.
├── app/
  ├── ...
├── config/
  ├── app.js
  ├── auth.js
  └── ...
├── database/
  ├── migrations/
  ├── seeds/
  └── factory.js
├── public/
├── resources/
  ├── ...
  └── views/
├── storage/
├── start/
  ├── app.js
  ├── kernel.js
  └── routes.js
├── test/
├── ace
├── server.js
└── package.json
```

- _app/_ Là thư mục chứa source code dự án của bạn, được autoloaded dưới tên namespace là App
- _config/_ Là thư mục chứa những file config của project
- _bootstrap/_ Nơi chứa những file khởi tạo và đăng ký app như app.js, events.js, extend.js
- _database/_ Chứa các file migration, seeder, hay factory để chúng ta khởi tạo dữ liệu với DB như trên laravel hay lưu trữ cơ sở dữ liệu SQLite.
- _public/_ Chứa các thành phần như ảnh, css, javascript.
- _resources/_ Nơi chứa views
- _storage/_ Nơi chứa các file biên dịch template, session hay log hệ thống.

## Route

Tất cả khai báo route của framework đều được định nghĩa trong file _start/routes.js_

- Basic route:

```
Route.get('/', () => 'Hello Adonis')

Route.get('posts', 'PostController.index')

Route.get(url, closure)
Route.post(url, closure)
Route.put(url, closure)
Route.patch(url, closure)
Route.delete(url, closure)
```

- Route Parameters:

```
Route.get('posts/:id', ({ params }) => {
  return `Post ${params.id}`
})
```

- Optional Parameters

```
Route.get('make/:drink?', ({ params }) => {
  // use Coffee as fallback when drink is not defined
  const drink = params.drink || 'Coffee'

  return `Will make ${drink} for you`
})
```

- Named Route

```
Route.get('users', closure).as('users.index')
```

Sử dụng như sau:

```
<!-- before -->
<a href="/users">List of users</a>

<!-- after -->
<a href="{{ route('users.index') }}">List of users</a>
```
- Route Resources

```
Route.resource('users', 'UserController')

// Equivalent of
Route.get('users', 'UserController.index').as('users.index')
Route.post('users', 'UserController.store').as('users.store')
Route.get('users/create', 'UserController.create').as('users.create')
Route.get('users/:id', 'UserController.show').as('users.show')
Route.put('users/:id', 'UserController.update').as('users.update')
Route.patch('users/:id', 'UserController.update')
Route.get('users/:id/edit', 'UserController.edit').as('users.edit')
Route.delete('users/:id', 'UserController.destroy').as('users.destroy')
```

Chúng ta có thể thấy rằng Adonis cũng hỗ trợ hết các method cơ bản như Route.get, Route.post, Route.put, Route.patch, Route.delete. Để custom một route cũng không phải là quá phức tạp:

```
const Route = use('Route')
// SINGLE VERB
Route.route('/', 'COPY', function * (request, response) { })

// MULTIPLE VERBS
Route.route('/', ['COPY', 'MOVE'], function * (request, response) { })
```

# Middleware

- Được đặt trong thư mục _app/Http/Middleware_
- Được khởi tạo bằng lệnh: 

```
adonis make:middleware CountryDetector
```

- Sử dụng Middleware cho Route:

```

# Single
Route.get('/authenticated', function * (request, response) {
  response.send('This route is authenticated')
}).middleware('auth')

#Multiple
Route.get('/secured', function * (request, response) {
  response.send('This route is authenticated')
}).middleware(['auth', 'custom'])
```

# Controllers

- Tạo mới một controller

```
> adonis make:controller User --type http # HTTP Controller
> adonis make:controller User --type ws   # WS Controller
> adonis make:controller Admin/User       # Will use an Admin subfolder
```

Câu lệnh trên sẽ tạo ra một controller mới trong thư mục _App/Controllers/{TYPE}_ với đoạn code mẫu như sau:

```
'use strict'

class UserController {
}

module.exports = UserController
```
- Sử dụng controller: Controller chỉ được access từ route. Định nghĩa 1 route cho controller như sau:

```
// app/Controllers/Http/UserController -> index()
Route.get(url, 'UserController.index')

// app/Controllers/Http/Admin/UserController -> store()
Route.post(url, 'Admin/UserController.store')

// app/MyOwnControllers/UserController -> index()
Route.post(url, 'App/MyOwnControllers/UserController.index')
```
Dĩ nhiên là chúng ta cần định nghĩa function index() trong UserController như sau:

```
'use strict'

class UserController {
  index ({ request, response }) {
    //
  }
}

module.exports = UserController
```

# Request

