# Hướng dẫn cách viết code Ruby (Ruby Style)

[RuboCop](https://github.com/bbatsov/rubocop) 

## Mục lục
* [Bố cục trình bày khi viết code / Source code layout](#b%E1%BB%91-c%E1%BB%A5c-tr%C3%ACnh-b%C3%A0y-khi-vi%E1%BA%BFt-code--source-code-layout)
* [Cú pháp / Syntax](#c%C3%BA-ph%C3%A1p--syntax)
* [Cách đặt tên / Naming](#c%C3%A1ch-%C4%91%E1%BA%B7t-t%C3%AAn--naming)
* [Viết chú thích / Comments](#vi%E1%BA%BFt-ch%C3%BA-th%C3%ADch--comments)
    * [Comment Annotations](#comment-annotations)
* [Lớp đối tượng và Module / Classes & Modules](#l%E1%BB%9Bp-%C4%91%E1%BB%91i-t%C6%B0%E1%BB%A3ng-v%C3%A0-module--classes--modules)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Numbers](#numbers)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Metaprogramming](#metaprogramming)
* [Misc](#misc)
* [Các công cụ / Tools](#c%C3%A1c-c%C3%B4ng-c%E1%BB%A5--tools)


## Bố cục trình bày khi viết code / Source code layout

> Gần đây, mọi người quả quyết rằng phong cách code của tất cả
> mọi người đều rất tởm và không thể đọc được, ngoại trừ chính họ.
> Nếu bỏ đi cụm từ "ngoại trừ chính họ" thì có lẽ rằng họ đã
> gần đúng :D... <br>
> -- Jerry Coffin

* <a name="utf-8"></a>
  Sử dụng chuẩn mã hóa `UTF-8` khi code.
<sup>[[link](#utf-8)]</sup>

* <a name="spaces-indentation"></a>
  Dùng hai (02) **Khoảng trắng**(**spaces**) cho mỗi tầng thụt đầu dòng (hay còn gọi là tab mềm (soft tabs)). Không sử dụng tab cứng (hard tabs).
<sup>[[link](#spaces-indentation)]</sup>

  ```Ruby
  # bad - four spaces
  def some_method
      do_something
  end

  # good
  def some_method
    do_something
  end
  ```

* <a name="crlf"></a>
  Sử dụng dấu cuối dòng theo Unix-style. (*Người dùng BSD/Solaris/Linux/OS X được kích hoạt sẵn, người dùng Windows cần phải để ý.)
<sup>[[link](#crlf)]</sup>

  * Nếu bạn đang dùng Git, bạn nên cấu hình như sau để chặn
    các dấu cuối dòng của Windows:

    ```bash
    $ git config --global core.autocrlf true
    ```

* <a name="no-semicolon"></a>
  Không dùng dấu chấm phẩy (`;`) để ngăn cách các câu lệnh và biểu thức,
  mà hãy viết mỗi câu 1 dòng.
<sup>[[link](#no-semicolon)]</sup>

  ```Ruby
  # bad
  puts 'foobar'; # Thừa dấu chấm phẩy

  puts 'foo'; puts 'bar' # hai câu lệnh trên cùng 1 dòng

  # good
  puts 'foobar'

  puts 'foo'
  puts 'bar'

  puts 'foo', 'bar' # câu lệnh này sẽ tự tách làm hai lệnh `puts`
  ```

* <a name="single-line-classes"></a>
  Nếu lớp không có nội dung, nên viết trên một dòng.
<sup>[[link](#single-line-classes)]</sup>

  ```Ruby
  # bad
  class FooError < StandardError
  end

  # okish
  class FooError < StandardError; end

  # good
  FooError = Class.new(StandardError)
  ```

* <a name="no-single-line-methods"></a>
  Tránh kiểu viết gom phương thức vào một dòng.
  Mặc dù ở một vài nơi nó vẫn được sử dụng, có một vài điểm đặc thù
  khiến người ta cảm thấy khó chịu khi gặp nó. Dù sao vẫn không nền viết
  nhiều hơn một biểu thức, câu lệnh trên một hàng.
<sup>[[link](#no-single-line-methods)]</sup>

  ```Ruby
  # bad
  def too_much; something; something_else; end

  # okish - dấu ; ngay sau tên hàm bắt buộc phải có
  def no_braces_method; body end

  # okish - dấu ; sau body thì không bắt buộc
  def no_braces_method; body; end

  # okish - đúng cú pháp, nhưng không có ; sau tên hàm khiến khó đọc hơn
  def some_method() body end

  # good
  def some_method
    body
  end
  ```

  Ngoại lệ: phương thức không có body thì nên viết trên một hàng

  ```Ruby
  # good
  def no_op; end
  ```

* <a name="spaces-operators"></a>
  Đặt khoảng trắng trước và sau toán tử ,
  sau dấu phẩy, dấu hai chấm và dấu chấm phẩy.
  Khoảng trắng có thể (hầu hết) là không bắt buộc, nhưng nó sẽ giúp
  code dễ đọc, dễ viết hơn.
<sup>[[link](#spaces-operators)]</sup>

  ```Ruby
  sum = 1 + 2
  a, b = 1, 2
  class FooError < StandardError; end
  ```

  Ngoại lệ duy nhất, liên quan đến các toán tử, là toán tử mũ:

  ```Ruby
  # bad
  e = M * c ** 2

  # good
  e = M * c**2
  ```

* <a name="spaces-braces"></a>
  KHÔNG DÙNG khoảng trắng sau `(`, `[` hay trước `]`, `)`.
  DÙNG khoảng trắng quanh `{` và trước `}`.
<sup>[[link](#spaces-braces)]</sup>

  ```Ruby
  # bad
  some( arg ).other
  [ 1, 2, 3 ].each{|e| puts e}

  # good
  some(arg).other
  [1, 2, 3].each { |e| puts e }
  ```

  `{` và `}` cần được làm rõ một chút, nó được dùng cho cả block, hash và
  dùng để nhúng biến vào string.

  Với hash, có hai cách viết được sử dụng:
  - Cách thứ nhất dễ đọc hơn, và thường được sử dụng nhiều hơn trong
  cộng đồng Ruby.
  - Cách thứ hai có điểm mạnh là nó dùng để phân biệt giữa block và hash.
  Cho dù bạn chọn cách nào, thì cũng nên theo một cách thôi.

  ```Ruby
  # good - khoảng trắng trước { và sau }
  { one: 1, two: 2 }

  # good - không có khoảng trắng trước { và sau }
  {one: 1, two: 2}
  ```

  Khi dùng để nhúng vào string, không nên dùng khoảng trắng giữa cặp ngoặc nhọn.

  ```Ruby
  # bad
  "From: #{ user.first_name }, #{ user.last_name }"

  # good
  "From: #{user.first_name}, #{user.last_name}"
  ```

* <a name="no-space-bang"></a>
  Không có khoảng trắng sau `!`.
<sup>[[link](#no-space-bang)]</sup>

  ```Ruby
  # bad
  ! something

  # good
  !something
  ```

* <a name="no-space-inside-range-literals"></a>
  Không dùng khoảng trắng khi khai báo khoảng (range).
<sup>[[link](#no-space-inside-range-literals)]</sup>

    ```Ruby
    # bad
    1 .. 3
    'a' ... 'z'

    # good
    1..3
    'a'...'z'
    ```

* <a name="indent-when-to-case"></a>
  `when` và `case` thụt đầu dòng cùng cấp. Style này được khuyến nghị trong cả
  "The Ruby Programming Language" và "Programming Ruby".
<sup>[[link](#indent-when-to-case)]</sup>

  ```Ruby
  # bad
  case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
  end

  # good
  case
  when song.name == 'Misty'
    puts 'Not again!'
  when song.duration > 120
    puts 'Too long!'
  when Time.now.hour > 21
    puts "It's too late"
  else
    song.play
  end
  ```

* <a name="indent-conditional-assignment"></a>
  Khi gán kết quả của một biểu thức cho một biến,
  cặp `case`, `when` hay `if`,`else` cũng phải cùng cấp.
<sup>[[link](#indent-conditional-assignment)]</sup>

  ```Ruby
  # bad - pretty convoluted
  kind = case year
  when 1850..1889 then 'Blues'
  when 1890..1909 then 'Ragtime'
  when 1910..1929 then 'New Orleans Jazz'
  when 1930..1939 then 'Swing'
  when 1940..1950 then 'Bebop'
  else 'Jazz'
  end

  result = if some_cond
    calc_something
  else
    calc_something_else
  end

  # good - it's apparent what's going on
  kind = case year
         when 1850..1889 then 'Blues'
         when 1890..1909 then 'Ragtime'
         when 1910..1929 then 'New Orleans Jazz'
         when 1930..1939 then 'Swing'
         when 1940..1950 then 'Bebop'
         else 'Jazz'
         end

  result = if some_cond
             calc_something
           else
             calc_something_else
           end

  # good (and a bit more width efficient)
  kind =
    case year
    when 1850..1889 then 'Blues'
    when 1890..1909 then 'Ragtime'
    when 1910..1929 then 'New Orleans Jazz'
    when 1930..1939 then 'Swing'
    when 1940..1950 then 'Bebop'
    else 'Jazz'
    end

  result =
    if some_cond
      calc_something
    else
      calc_something_else
    end
  ```

* <a name="empty-lines-between-methods"></a>
  Thêm một dòng trống giữa các phương thức, và các nhóm xử lý logic.
<sup>[[link](#empty-lines-between-methods)]</sup>

  ```Ruby
  def some_method
    data = initialize(options)

    data.manipulate!

    data.result
  end

  def some_method
    result
  end
  ```

* <a name="no-trailing-params-comma"></a>
  KHÔNG DÙNG dấu phẩy sau tham số cuối cùng khi khai báo phương thức,
  đặc biệt khi những tham số không nằm trên những dòng riêng biệt.
<sup>[[link](#no-trailing-params-comma)]</sup>

  ```Ruby
  # bad - cách làm này giúp dễ thêm/xóa/di chuyển tham số,
  # nhưng không khuyến khích dùng
  some_method(
               size,
               count,
               color,
             )

  # bad
  some_method(size, count, color, )

  # good
  some_method(size, count, color)
  ```

* <a name="spaces-around-equals"></a>
  Dùng khoảng trắng quanh toán tử `=` khi gán giá trị mặc định:
<sup>[[link](#spaces-around-equals)]</sup>

  ```Ruby
  # bad
  def some_method(arg1=:default, arg2=nil, arg3=[])
    # do something...
  end

  # good
  def some_method(arg1 = :default, arg2 = nil, arg3 = [])
    # do something...
  end
  ```

  Trong khi một số sách về Ruby khuyên dùng cách một, nhưng cộng đồng lại
  thích dùng cách hai nhiều hơn.

* <a name="no-trailing-backslash"></a>
  Tránh dùng dấu ngắt dòng `\` khi không bắt buộc. Thực tế, chỉ dùng nó khi
  cần ngắt dòng string thôi.
<sup>[[link](#no-trailing-backslash)]</sup>

  ```Ruby
  # bad
  result = 1 - \
           2

  # good (but still ugly as hell)
  result = 1 \
           - 2

  long_string = 'First part of the long string' \
                ' and second part of the long string'
  ```

* <a name="consistent-multi-line-chains"></a>
    Với cách gọi phương thức nhiều lần liên tiếp, có hai kiểu thông dụng,
    kiểu nào cũng được cả:
    `.` ở đầu dòng mới (Phương án A) `.` ở cuối dòng cũ (Phương án B).
    Mặc dù dùng kiểu nào thì cũng nên dùng nhất quán một kiểu.
<sup>[[link](#consistent-multi-line-chains)]</sup>

  * **(Phương án A)**
    ```Ruby
    one.two.three
       .four
    ```

  * **(Phương án B)**
    ```Ruby
    one.two.three.
       four
    ```

  Xem thêm về cuộc thảo luận chọn cách nào:
  [tại đây](https://github.com/bbatsov/ruby-style-guide/pull/176).

* <a name="no-double-indent"></a>
    Khi gọi phương thức có nhiều đối số, nên xuống dòng, và các đối số này
    cần thụt đầu dòng bằng nhau và ở cùng cấp với đối số đầu tiên.
    Nếu đối số này dài thì có thể dùng cách thụt đầu dòng bình thường.
<sup>[[link](#no-double-indent)]</sup>

  ```Ruby
  # mẫu (dòng quá dài)
  def send_mail(source)
    Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
  end

  # bad (thụt đầu dòng hai lần)
  def send_mail(source)
    Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
  end

  # good
  def send_mail(source)
    Mailer.deliver(to: 'bob@example.com',
                   from: 'us@example.com',
                   subject: 'Important message',
                   body: source.text)
  end

  # good (thụt đầu dòng bình thường)
  def send_mail(source)
    Mailer.deliver(
      to: 'bob@example.com',
      from: 'us@example.com',
      subject: 'Important message',
      body: source.text
    )
  end
  ```

* <a name="align-multiline-arrays"></a>
  Nếu mảng có nhiều phần tử, nên đưa xuống dòng và thụt đầu dòng cùng cấp.
<sup>[[link](#align-multiline-arrays)]</sup>

  ```Ruby
  # bad
  menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
    'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']

  # good
  menu_item = [
    'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
    'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam'
  ]

  # good
  menu_item =
    ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
     'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
  ```

* <a name="underscores-in-numerics"></a>
  Với số lớn, thêm dấu gạch dưới `_` cho dễ đọc.
<sup>[[link](#underscores-in-numerics)]</sup>

  ```Ruby
  # bad - có bao nhiêu số `0`?
  num = 1000000000

  # good - dễ đọc hơn hẳn đúng không :P
  num = 1_000_000_000
  ```

* <a name="rdoc-conventions"></a>
    Sử dụng [Rdoc][rdoc] và convention của nó để làm API documentation.
    KHÔNG đặt khoảng trắng giữa khối comment và từ khóa `def`.
<sup>[[link](#rdoc-conventions)]</sup>

* <a name="80-character-limits"></a>
  Giới hạn dòng ở 80 ký tự.
<sup>[[link](#80-character-limits)]</sup>

* <a name="no-trailing-whitespace"></a>
  Tránh dùng khoảng trắng thừa (thường gặp ở cuối dòng).
<sup>[[link](#no-trailing-whitespace)]</sup>

* <a name="newline-eof"></a>
  Cuối file nên có thêm một dòng trống.
<sup>[[link](#newline-eof)]</sup>

* <a name="no-block-comments"></a>
    KHÔNG DÙNG block comments.
<sup>[[link](#no-block-comments)]</sup>

  ```Ruby
  # bad
  =begin
  comment line
  another comment line
  =end

  # good
  # comment line
  # another comment line
  ```

## Cú pháp / Syntax

* <a name="double-colons"></a>
    CHỈ sử dụng `::` cho hằng số (của cả classes và modules)
    và các hàm khởi tạo (constructors) (vd: `Array()` hoặc `Nokogiri::HTML()`).
    KHÔNG sử dụng `::` cho các lời gọi hàm thông thường.
<sup>[[link](#double-colons)]</sup>

  ```Ruby
  # bad
  SomeClass::some_method
  some_object::some_method

  # good
  SomeClass.some_method
  some_object.some_method
  SomeModule::SomeClass::SOME_CONST
  SomeModule::SomeClass()
  ```

* <a name="method-parens"></a>
    Chỉ dùng `()` khi khai báo phương thức có tham số.
<sup>[[link](#method-parens)]</sup>

   ```Ruby
   # bad
   def some_method()
     # body omitted
   end

   # good
   def some_method
     # body omitted
   end

   # bad
   def some_method_with_parameters param1, param2
     # body omitted
   end

   # good
   def some_method_with_parameters(param1, param2)
     # body omitted
   end
   ```

* <a name="optional-arguments"></a>
    Nếu phương thức có tham số mặc định, đặt nó ở cuối cùng.
    Nếu đặt trước một tham số khác thì hậu quả thật là khôn lường :3
    Ruby nó hành xử vui lắm :P
<sup>[[link](#optional-arguments)]</sup>

  ```Ruby
  # bad
  def some_method(a = 1, b = 2, c, d)
    puts "#{a}, #{b}, #{c}, #{d}"
  end

  some_method('w', 'x') # => '1, 2, w, x'
  some_method('w', 'x', 'y') # => 'w, 2, x, y'
  some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'

  # good
  def some_method(c, d, a = 1, b = 2)
    puts "#{a}, #{b}, #{c}, #{d}"
  end

  some_method('w', 'x') # => '1, 2, w, x'
  some_method('w', 'x', 'y') # => 'y, 2, w, x'
  some_method('w', 'x', 'y', 'z') # => 'y, z, w, x'
  ```

* <a name="parallel-assignment"></a>
    TRÁNH khai báo biến song song, sẽ gây khó khăn khi đọc.
    Khai báo biến song song chỉ dùng khi trả kết quả phương thức,
    dùng với toán tử splat, hay dùng để hoán đổi giá trị hai biến.
    NÊN khai báo mỗi biến một hàng.
<sup>[[link](#parallel-assignment)]</sup>

  ```Ruby
  # bad
  a, b, c, d = 'foo', 'bar', 'baz', 'foobar'

  # good
  a = 'foo'
  b = 'bar'
  c = 'baz'
  d = 'foobar'

  # good - hoán đổi giá trị hai biến
  a = 'foo'
  b = 'bar'

  a, b = b, a
  puts a # => 'bar'
  puts b # => 'foo'

  # good - method return
  def multi_return
    [1, 2]
  end

  first, second = multi_return

  # good - dùng với splat
  first, *list = [1, 2, 3, 4] # first => 1, list => [2, 3, 4]

  hello_array = *'Hello' # => ["Hello"]

  a = *(1..3) # => [1, 2, 3]
  ```

* <a name="trailing-underscore-variables"></a>
  Tránh dùng dấu gạch dưới thừa thãi trong khai báo song song.
  Nên dùng `_` với tên biến cho rõ nghĩa.
  `_` cần thiết khi có một biến `splat`, và biến này thì không cần `_`.
<sup>[[link]](#trailing-underscore-variables)</sup>

  ```Ruby
  # bad
  foo = 'one,two,three,four,five'
  # Phép gán không cần thiết thì không cung cấp bất kỳ thông tin hữu ích nào
  first, second, _ = foo.split(',')
  first, _, _ = foo.split(',')
  first, *_ = foo.split(',')


  # good
  foo = 'one,two,three,four,five'
  # Dấu `_` được dùng khi ta cần lấy hết ra, trừ phần tử cuối cùng.
  *beginning, _ = foo.split(',')
  *beginning, something, _ = foo.split(',')

  a, = foo.split(',')
  a, b, = foo.split(',')
  # Việc gán cho biến không sử dụng là không cần thiết,
  # nhưng nó cung cấp thông tin hữu ích cho ta.
  first, _second = foo.split(',')
  first, _second, = foo.split(',')
  first, *_ending = foo.split(',')
  ```

* <a name="no-for-loops"></a>
    Đừng dùng `for`, trừ khi có lý do chính đáng. Thay vào đó hãy dùng vòng lặp.
    `for` là một dạng của `each`, nhưng `for` không hỗ trợ scope (`each` thì có)
    và biến trong `for` thì có thể truy cập từ bên ngoài block.
<sup>[[link](#no-for-loops)]</sup>

  ```Ruby
  arr = [1, 2, 3]

  # bad
  for elem in arr do
    puts elem
  end

  # ra khỏi vòng `for` ta vẫn truy cập biến `elem` được
  elem # => 3

  # good
  arr.each { |elem| puts elem }

  elem # => NameError: undefined local variable or method `elem'
  ```

* <a name="no-then"></a>
  KHÔNG dùng `then` trong `if/unless` nhiều cấp.
<sup>[[link](#no-then)]</sup>

  ```Ruby
  # bad
  if some_condition then
    # body omitted
  end

  # good
  if some_condition
    # body omitted
  end
  ```

* <a name="same-line-condition"></a>
  LUÔN đặt điều kiện cùng cấp với `if`/`unless`.
<sup>[[link](#same-line-condition)]</sup>

  ```Ruby
  # bad
  if
    some_condition
    do_something
    do_something_else
  end

  # good
  if some_condition
    do_something
    do_something_else
  end
  ```

* <a name="ternary-operator"></a>
  NÊN dùng toán tử ba ngôi (`?:`) hơn là cấu trúc `if/then/else/end`.
  Nó thông dụng hơn và giúp code ngắn gọn súc tích hơn.
<sup>[[link](#ternary-operator)]</sup>

  ```Ruby
  # bad
  result = if some_condition then something else something_else end

  # good
  result = some_condition ? something : something_else
  ```

* <a name="no-nested-ternary"></a>
  KHÔNG nên dùng toán tử ba ngôi lồng nhau.
  Trong trường hợp này thì nên dùng `if/else`.
<sup>[[link](#no-nested-ternary)]</sup>

  ```Ruby
  # bad
  some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

  # good
  if some_condition
    nested_condition ? nested_something : nested_something_else
  else
    something_else
  end
  ```

* <a name="no-semicolon-ifs"></a>
  KHÔNG dùng `if x; ...`. Trong trường hợp này thì dùng toán tử ba ngôi.
<sup>[[link](#no-semicolon-ifs)]</sup>

  ```Ruby
  # bad
  result = if some_condition; something else something_else end

  # good
  result = some_condition ? something : something_else
  ```

* <a name="use-if-case-returns"></a>
  Để ý rằng `if` case `case` có trả về kết quả.
<sup>[[link](#use-if-case-returns)]</sup>

  ```Ruby
  # bad
  if condition
    result = x
  else
    result = y
  end

  # good
  result =
    if condition
      x
    else
      y
    end
  ```

* <a name="one-line-cases"></a>
  DÙNG `when x then ...` cho một-dòng cases. Cú pháp `when x:
  ...` bị remove từ Ruby 1.9.
<sup>[[link](#one-line-cases)]</sup>

* <a name="no-when-semicolons"></a>
  KHÔNG dùng `when x; ...`. Xem mục ở trên.
<sup>[[link](#no-when-semicolons)]</sup>

* <a name="bang-not-not"></a>
  DÙNG `!` thay cho `not`.
<sup>[[link](#bang-not-not)]</sup>

  ```Ruby
  # bad - parentheses are required because of op precedence
  x = (not something)

  # good
  x = !something
  ```

* <a name="no-bang-bang"></a>
  Tránh dùng `!!`.
<sup>[[link](#no-bang-bang)]</sup>

  ```Ruby
  # bad
  x = 'test'
  # obscure nil check
  if !!x
    # body omitted
  end

  x = false
  # double negation is useless on booleans
  !!x # => false

  # good
  x = 'test'
  unless x.nil?
    # body omitted
  end
  ```

* <a name="no-and-or-or"></a>
  KHÔNG dùng từ khóa `and` và `or`. Thay vào đó hãy dùng `&&` và `||`.
<sup>[[link](#no-and-or-or)]</sup>

  ```Ruby
  # bad
  # boolean expression
  if some_condition and some_other_condition
    do_something
  end

  # control flow
  document.saved? or document.save!

  # good
  # boolean expression
  if some_condition && some_other_condition
    do_something
  end

  # control flow
  document.saved? || document.save!
  ```

* <a name="no-multiline-ternary"></a>
  Tránh dùng `?:` nhiều dòng; thay vào đó hãy dùng `if/unless`.
<sup>[[link](#no-multiline-ternary)]</sup>

* <a name="if-as-a-modifier"></a>
  Dùng `if/unless` cho single-line body.
<sup>[[link](#if-as-a-modifier)]</sup>

  ```Ruby
  # bad
  if some_condition
    do_something
  end

  # good
  do_something if some_condition

  # another good option
  some_condition && do_something
  ```

* <a name="no-multiline-if-modifiers"></a>
  Tránh dùng `if/unless` ở cuối block khi không cần thiết.
<sup>[[link](#no-multiline-if-modifiers)]</sup>

  ```Ruby
  # bad
  10.times do
    # multi-line body omitted
  end if some_condition

  # good
  if some_condition
    10.times do
      # multi-line body omitted
    end
  end
  ```

* <a name="no-nested-modifiers"></a>
  Tránh dùng `if/unless/while/until` lồng nhau.
  Nên dùng chung `&&/||` nếu thích hợp.
<sup>[[link](#no-nested-modifiers)]</sup>

  ```Ruby
  # bad
  do_something if other_condition if some_condition

  # good
  do_something if some_condition && other_condition
  ```

* <a name="unless-for-negatives"></a>
  Nếu điều kiện là `false`, dùng `unless` thay vì `if not`.
  Hoặc dùng `||` cũng được.
<sup>[[link](#unless-for-negatives)]</sup>

  ```Ruby
  # bad
  do_something if !some_condition

  # bad
  do_something if not some_condition

  # good
  do_something unless some_condition

  # another good option
  some_condition || do_something
  ```

* <a name="no-else-with-unless"></a>
  KHÔNG dùng `unless` cùng với `else`. Thay vào đó hãy viết với biểu thức `true`.
<sup>[[link](#no-else-with-unless)]</sup>

  ```Ruby
  # bad
  unless success?
    puts 'failure'
  else
    puts 'success'
  end

  # good
  if success?
    puts 'success'
  else
    puts 'failure'
  end
  ```

* <a name="no-parens-if"></a>
  KHÔNG dùng cặp ngoặc tròn `()` với `if/unless/while/until`.
<sup>[[link](#no-parens-if)]</sup>

  ```Ruby
  # bad
  if (x > 10)
    # body omitted
  end

  # good
  if x > 10
    # body omitted
  end
  ```

Tuy nhiên, có một ngoại lệ, có tên là [safe assignment in
condition](#safe-assignment-in-condition).

* <a name="no-multiline-while-do"></a>
  KHÔNG dùng `while/until condition do` cho `while/until` nhiều dòng.
<sup>[[link](#no-multiline-while-do)]</sup>

  ```Ruby
  # bad
  while x > 5 do
    # body omitted
  end

  until x > 5 do
    # body omitted
  end

  # good
  while x > 5
    # body omitted
  end

  until x > 5
    # body omitted
  end
  ```

* <a name="while-as-a-modifier"></a>
  Đặt `while/until` ở sau trong trường hợp body một dòng.
<sup>[[link](#while-as-a-modifier)]</sup>

  ```Ruby
  # bad
  while some_condition
    do_something
  end

  # good
  do_something while some_condition
  ```

* <a name="until-for-negatives"></a>
  Ưu tiên `until` hơn `while` cho biểu thức `false`.
<sup>[[link](#until-for-negatives)]</sup>

  ```Ruby
  # bad
  do_something while !some_condition

  # good
  do_something until some_condition
  ```

* <a name="infinite-loop"></a>
  Dùng `Kernel#loop` thay vì `while/until` khi cần lặp vô hạn.
<sup>[[link](#infinite-loop)]</sup>

    ```ruby
    # bad
    while true
      do_something
    end

    until false
      do_something
    end

    # good
    loop do
      do_something
    end
    ```

* <a name="loop-with-break"></a>
  Dùng `Kernel#loop` cùng với `break` thay vì `begin/end/until` hay
  `begin/end/while` kiểu lặp trước, kiểm tra sau.
<sup>[[link](#loop-with-break)]</sup>

  ```Ruby
  # bad
  begin
    puts val
    val += 1
  end while val < 0

  # good
  loop do
    puts val
    val += 1
    break unless val < 0
  end
  ```

* <a name="no-dsl-parens"></a>
  Với các phương thức mặc định của DSL (vd: Rake, Rails, RSpec),
  hay các phương thức có chứa "từ khóa" trong Ruby
  (vd: `attr_reader`, `puts`) và các phương thức truy cập thuộc tính thì không dùng `()`.
  Còn lại là dùng hết.
<sup>[[link](#no-dsl-parens)]</sup>

  ```Ruby
  class Person
    # bad
    attr_reader(:name, :age)
    # good
    attr_reader :name, :age

    # body omitted
  end

  # bad
  temperance = Person.new 'Temperance', 30
  # good
  temperance = Person.new('Temperance', 30)

  # bad
  puts(temperance.age)
  # good
  puts temperance.age

  # bad
  x = Math.sin y
  # good
  x = Math.sin(y)

  # bad
  array.delete e
  # good
  array.delete(e)

  # bad
  expect(bowling.score).to eq 0
  # good
  expect(bowling.score).to eq(0)
  ```

* <a name="no-braces-opts-hash"></a>
  Nếu hash đã tường minh rồi thì không cần cặp `{}` nữa.
<sup>[[link](#no-braces-opts-hash)]</sup>

  ```Ruby
  # bad
  user.set({ name: 'John', age: 45, permissions: { read: true } })

  # good
  user.set(name: 'John', age: 45, permissions: { read: true })
  ```

* <a name="no-dsl-decorating"></a>
  Nếu phương thức hệ thống thì có thể bỏ luôn cả `()` và `{}`.
<sup>[[link](#no-dsl-decorating)]</sup>

  ```Ruby
  class Person < ActiveRecord::Base
    # bad
    validates(:name, { presence: true, length: { within: 1..10 } })

    # good
    validates :name, presence: true, length: { within: 1..10 }
  end
  ```

* <a name="no-args-no-parens"></a>
  Gọi phương thức không có tham số thì bỏ luôn `()`.
<sup>[[link](#no-args-no-parens)]</sup>

  ```Ruby
  # bad
  Kernel.exit!()
  2.even?()
  fork()
  'test'.upcase()

  # good
  Kernel.exit!
  2.even?
  fork
  'test'.upcase
  ```

* <a name="single-action-blocks"></a>
  Nếu block chỉ làm một việc thì ưu tiên dùng shorthand.
<sup>[[link](#single-action-blocks)]</sup>

  ```Ruby
  # bad
  names.map { |name| name.upcase }

  # good
  names.map(&:upcase)
  ```

* <a name="single-line-blocks"></a>
  Ưu tiên `{...}` hơn `do...end` cho block một dòng.
  Tránh dùng `{...}` cho block nhiều dòng.
  Luôn dùng `do...end` cho khối điều khiển và khai báo phương thức.
<sup>[[link](#single-line-blocks)]</sup>

  ```Ruby
  names = %w(Bozhidar Steve Sarah)

  # bad
  names.each do |name|
    puts name
  end

  # good
  names.each { |name| puts name }

  # bad
  names.select do |name|
    name.start_with?('S')
  end.map { |name| name.upcase }

  # good
  names.select { |name| name.start_with?('S') }.map(&:upcase)
  ```

* <a name="block-argument"></a>
  Cân nhắc sử dụng đối số cho block tường minh để tránh việc viết block 
  mà chỉ truyền các đối số của nó cho một block..
<sup>[[link](#block-argument)]</sup>

  ```Ruby
  require 'tempfile'

  # bad
  def with_tmp_dir
    Dir.mktmpdir do |tmp_dir|
      Dir.chdir(tmp_dir) { |dir| yield dir }  # block just passes arguments
    end
  end

  # good
  def with_tmp_dir(&block)
    Dir.mktmpdir do |tmp_dir|
      Dir.chdir(tmp_dir, &block)
    end
  end

  with_tmp_dir do |dir|
    puts "dir is accessible as a parameter and pwd is set: #{dir}"
  end
  ```

* <a name="no-explicit-return"></a>
  Tránh dùng `return`, chỉ nên dùng trong luồng điều khiển `if..else` chẳng hạn.
<sup>[[link](#no-explicit-return)]</sup>

  ```Ruby
  # bad
  def some_method(some_arr)
    return some_arr.size
  end

  # good
  def some_method(some_arr)
    some_arr.size
  end
  ```

* <a name="no-self-unless-required"></a>
  Tránh dùng `self` khi không bắt buộc.
  Chỉ dùng nó khi cần ghi giá trị vào biến.
<sup>[[link](#no-self-unless-required)]</sup>

  ```Ruby
  # bad
  def ready?
    if self.last_reviewed_at > self.last_updated_at
      self.worker.update(self.content, self.options)
      self.status = :in_progress
    end
    self.status == :verified
  end

  # good
  def ready?
    if last_reviewed_at > last_updated_at
      worker.update(content, options)
      self.status = :in_progress
    end
    status == :verified
  end
  ```

* <a name="no-shadowing"></a>
  As a corollary, avoid shadowing methods with local variables unless they are
  both equivalent.
<sup>[[link](#no-shadowing)]</sup>

  ```Ruby
  class Foo
    attr_accessor :options

    # ok
    def initialize(options)
      self.options = options
      # both options and self.options are equivalent here
    end

    # bad
    def do_something(options = {})
      unless options[:when] == :later
        output(self.options[:message])
      end
    end

    # good
    def do_something(params = {})
      unless params[:when] == :later
        output(options[:message])
      end
    end
  end
  ```

* <a name="safe-assignment-in-condition"></a>
  Không dùng kết quả của phép gán `=` trong biểu thức điều kiện nếu như
  không có cặp ngoặc tròn.
  Xem thêm *safe assignment in
  condition*.
<sup>[[link](#safe-assignment-in-condition)]</sup>

  ```Ruby
  # bad (+ a warning)
  if v = array.grep(/foo/)
    do_something(v)
    # some code
  end

  # good (MRI would still complain, but RuboCop won't)
  if (v = array.grep(/foo/))
    do_something(v)
    # some code
  end

  # good
  v = array.grep(/foo/)
  if v
    do_something(v)
    # some code
  end
  ```

* <a name="self-assignment"></a>
  Dùng kiểu gán rút gọn khi có thể
<sup>[[link](#self-assignment)]</sup>

  ```Ruby
  # bad
  x = x + y
  x = x * y
  x = x**y
  x = x / y
  x = x || y
  x = x && y

  # good
  x += y
  x *= y
  x **= y
  x /= y
  x ||= y
  x &&= y
  ```

* <a name="double-pipe-for-uninit"></a>
  Dùng `||=` để khởi tạo giá trị khi biến đó chưa được khởi tạo
<sup>[[link](#double-pipe-for-uninit)]</sup>

  ```Ruby
  # bad
  name = name ? name : 'Bozhidar'

  # bad
  name = 'Bozhidar' unless name

  # good - set name to 'Bozhidar', only if it's nil or false
  name ||= 'Bozhidar'
  ```

* <a name="no-double-pipes-for-bools"></a>
  Không dùng `||=` để khởi tạo biến boolean. (Xét trường hợp biến đó
    mang giá trị `false` sẵn rồi.)
<sup>[[link](#no-double-pipes-for-bools)]</sup>

  ```Ruby
  # bad - would set enabled to true even if it was false
  enabled ||= true

  # good
  enabled = true if enabled.nil?
  ```

* <a name="double-amper-preprocess"></a>
  Dùng `&&=` để tiền xử lý biến khi nó có thể tồn tại hoặc không.
  Việc dùng `&&=` sẽ chỉ thay đổi giá trị của biến khi nó đã tồn tại.
  Không cần thiết phải kiểm tra xem biến tồn tại hay chưa.
<sup>[[link](#double-amper-preprocess)]</sup>

  ```Ruby
  # bad
  if something
    something = something.downcase
  end

  # bad
  something = something ? something.downcase : nil

  # ok
  something = something.downcase if something

  # good
  something = something && something.downcase

  # better
  something &&= something.downcase
  ```

* <a name="no-case-equality"></a>
  Tránh việc ngầm định việc sử dụng của toán tử `===`.
<sup>[[link](#no-case-equality)]</sup>

  ```Ruby
  # bad
  Array === something
  (1..100) === 7
  /something/ === some_string

  # good
  something.is_a?(Array)
  (1..100).include?(7)
  some_string =~ /something/
  ```

* <a name="eql"></a>
  Tránh dùng `eql?`, hãy dùng `==`.
<sup>[[link](#eql)]</sup>

  ```Ruby
  # bad - eql? is the same as == for strings
  'ruby'.eql? some_str

  # good
  'ruby' == some_str
  1.0.eql? x # eql? makes sense here if want to differentiate between Integer and Float 1
  ```

* <a name="no-cryptic-perlisms"></a>
  Tránh sử dụng các biến đặc biệt kiểu Perl (như `$:`, `$;`,.. ). Chúng khá là khó hiểu
  và cách sử dụng chúng trong bất cứ tình huống nào ngoài các đoạn code một dòng thì không
  được khuyến khích. Sử dụng các alias thân thiện với con người được cung cấp bởi thư viện 
  `tiếng Anh`
<sup>[[link](#no-cryptic-perlisms)]</sup>

  ```Ruby
  # bad
  $:.unshift File.dirname(__FILE__)

  # good
  require 'English'
  $LOAD_PATH.unshift File.dirname(__FILE__)
  ```

* <a name="parens-no-spaces"></a>
  Không dùng khoảng trắng giữa tên hàm và dấu ngoặc mở `(`
<sup>[[link](#parens-no-spaces)]</sup>

  ```Ruby
  # bad
  f (3 + 2) + 1

  # good
  f(3 + 2) + 1
  ```

* <a name="parens-as-args"></a>
  Nếu đối số đầu tiên của phương thức có dùng ngoặc tròn
  thì lời gọi phương thức phải có ngoặc tròn. Vd: `f((3 + 2) + 1)`.
<sup>[[link](#parens-as-args)]</sup>

* <a name="always-warn-at-runtime"></a>
  Luôn chạy trình biên dịch Ruby với tuy chọn `-w`,
  nó sẽ cảnh báo nếu ta quên một quy tắc nào ở trên.
<sup>[[link](#always-warn-at-runtime)]</sup>

* <a name="no-nested-methods"></a>
  Không sử dụng các định nghĩa phương thức lồng nhau, thay vào đó hãy dùng lambda.
  Các định nghĩa phương thức lồng nhau thật ra đưa các phương thức trong cùng
  phạm vi (ví dụ lớp) như là một phương thức bên ngoài. Hơn nữa, các phương thức
  lồng sẽ bị định nghĩa lại mỗi lần phương thức chứa nó được gọi.
<sup>[[link](#no-nested-methods)]</sup>

  ```Ruby
  # bad
  def foo(x)
    def bar(y)
      # body omitted
    end

    bar(x)
  end

  # good - the same as the previous, but no bar redefinition on every foo call
  def bar(y)
    # body omitted
  end

  def foo(x)
    bar(x)
  end

  # also good
  def foo(x)
    bar = ->(y) { ... }
    bar.call(x)
  end
  ```

* <a name="lambda-multi-line"></a>
  Dùng cú pháp lambda mới cho block có một dòng.
  Nếu nhiều dòng thì dùng từ khóa `lambda`.
<sup>[[link](#lambda-multi-line)]</sup>

  ```Ruby
  # bad
  l = lambda { |a, b| a + b }
  l.call(1, 2)

  # correct, but looks extremely awkward
  l = ->(a, b) do
    tmp = a * 7
    tmp * b / 50
  end

  # good
  l = ->(a, b) { a + b }
  l.call(1, 2)

  l = lambda do |a, b|
    tmp = a * 7
    tmp * b / 50
  end
  ```

* <a name="stabby-lambda-with-args"></a>
Khi dùng lambda với biến, nên đặt biến trong ngoặc tròn.
<sup>[[link](#stabby-lambda-with-args)]</sup>

  ```Ruby
  # bad
  l = ->x, y { something(x, y) }

  # good
  l = ->(x, y) { something(x, y) }
  ```

* <a name="stabby-lambda-no-args"></a>
    Lambda không có tham số thì bỏ cặp ngoặc tròn đi.
<sup>[[link](#stabby-lambda-no-args)]</sup>

  ```Ruby
  # bad
  l = ->() { something }

  # good
  l = -> { something }
  ```

* <a name="proc"></a>
  Ưu tiên `proc` hơn `Proc.new`.
<sup>[[link](#proc)]</sup>

  ```Ruby
  # bad
  p = Proc.new { |n| puts n }

  # good
  p = proc { |n| puts n }
  ```

* <a name="proc-call"></a>
  Ưu tiên `proc.call()` hơn `proc[]` hay `proc.()` cho cả lambdas và procs.
<sup>[[link](#proc-call)]</sup>

  ```Ruby
  # bad - looks similar to Enumeration access
  l = ->(v) { puts v }
  l[1]

  # also bad - uncommon syntax
  l = ->(v) { puts v }
  l.(1)

  # good
  l = ->(v) { puts v }
  l.call(1)
  ```

* <a name="underscore-unused-vars"></a>
  Dùng tiền tố `_` cho biến không dùng trong block và biến cục bộ.
  Hoặc chỉ dùng `_` cũng được.
<sup>[[link](#underscore-unused-vars)]</sup>

  ```Ruby
  # bad
  result = hash.map { |k, v| v + 1 }

  def something(x)
    unused_var, used_var = something_else(x)
    # some code
  end

  # good
  result = hash.map { |_k, v| v + 1 }

  def something(x)
    _unused_var, used_var = something_else(x)
    # some code
  end

  # good
  result = hash.map { |_, v| v + 1 }

  def something(x)
    _, used_var = something_else(x)
    # some code
  end
  ```

* <a name="global-stdout"></a>
  Dùng `$stdout/$stderr/$stdin` thay cho `STDOUT/STDERR/STDIN`.
  `STDOUT/STDERR/STDIN` là những hằng số, và ta có thẻ thay đổi mấy hằng số đó.
<sup>[[link](#global-stdout)]</sup>

* <a name="warn"></a>
    Dùng `warn` thay cho `$stderr.puts`. Dùng `warn` sẽ rõ nghĩa hơn.
    Đồng thời cho phép ta bỏ qua warning nếu cần (bằng cách set warn level về 0
    thông qua `-W0`).
<sup>[[link](#warn)]</sup>

* <a name="sprintf"></a>
  Ưu tiên dùng `sprintf` và bí danh `format` của nó hơn là phương thức `String#%`.
<sup>[[link](#sprintf)]</sup>

  ```Ruby
  # bad
  '%d %d' % [20, 10]
  # => '20 10'

  # good
  sprintf('%d %d', 20, 10)
  # => '20 10'

  # good
  sprintf('%{first} %{second}', first: 20, second: 10)
  # => '20 10'

  format('%d %d', 20, 10)
  # => '20 10'

  # good
  format('%{first} %{second}', first: 20, second: 10)
  # => '20 10'
  ```

* <a name="array-join"></a>
  Ưu tiên dùng `Array#join` hơn là `Array#*` với tham số là string.
<sup>[[link](#array-join)]</sup>

  ```Ruby
  # bad
  %w(one two three) * ', '
  # => 'one, two, three'

  # good
  %w(one two three).join(', ')
  # => 'one, two, three'
  ```

* <a name="array-coercion"></a>
  Dùng `Array()` thay vì kiểm tra `Array` một cách tường minh hay `[*var]`,
<sup>[[link](#array-coercion)]</sup>

  ```Ruby
  # bad
  paths = [paths] unless paths.is_a? Array
  paths.each { |path| do_something(path) }

  # bad (always creates a new Array instance)
  [*paths].each { |path| do_something(path) }

  # good (and a bit more readable)
  Array(paths).each { |path| do_something(path) }
  ```

* <a name="ranges-or-between"></a>
  Khi xử lý logic dải mà phức tạp, dùng dải (range) hoặc `Comparable#between?`
  khi có thể.
<sup>[[link](#ranges-or-between)]</sup>

  ```Ruby
  # bad
  do_something if x >= 1000 && x <= 2000

  # good
  do_something if (1000..2000).include?(x)

  # good
  do_something if x.between?(1000, 2000)
  ```

* <a name="predicate-methods"></a>
  Ưu tiên dùng các phương thức có sẵn hơn là so sánh tường minh với `==`.
  So sánh với số cũng chấp nhận được.
<sup>[[link](#predicate-methods)]</sup>

  ```Ruby
  # bad
  if x % 2 == 0
  end

  if x % 2 == 1
  end

  if x == nil
  end

  # good
  if x.even?
  end

  if x.odd?
  end

  if x.nil?
  end

  if x.zero?
  end

  if x == 0
  end
  ```

* <a name="no-non-nil-checks"></a>
  Khi kiểm tra điều kiện thì không cần kiểm tra với `nil` trừ khi làm với boolean.
<sup>[[link](#no-non-nil-checks)]</sup>

    ```ruby
    # bad
    do_something if !something.nil?
    do_something if something != nil

    # good
    do_something if something

    # good - dealing with a boolean
    def value_set?
      !@some_boolean.nil?
    end
    ```

* <a name="no-BEGIN-blocks"></a>
  Avoid the use of `BEGIN` blocks.
<sup>[[link](#no-BEGIN-blocks)]</sup>

* <a name="no-END-blocks"></a>
  Do not use `END` blocks. Use `Kernel#at_exit` instead.
<sup>[[link](#no-END-blocks)]</sup>

  ```ruby
  # bad
  END { puts 'Goodbye!' }

  # good
  at_exit { puts 'Goodbye!' }
  ```

* <a name="no-flip-flops"></a>
  Tránh dùng flip-flops.
<sup>[[link](#no-flip-flops)]</sup>

* <a name="no-nested-conditionals"></a>
  Tránh dùng điều kiện lồng. Thay vào đó hãy kiểm tra dữ liệu trước.
<sup>[[link](#no-nested-conditionals)]</sup>

  ```Ruby
  # bad
  def compute_thing(thing)
    if thing[:foo]
      update_with_bar(thing)
      if thing[:foo][:bar]
        partial_compute(thing)
      else
        re_compute(thing)
      end
    end
  end

  # good
  def compute_thing(thing)
    return unless thing[:foo]
    update_with_bar(thing[:foo])
    return re_compute(thing) unless thing[:foo][:bar]
    partial_compute(thing)
  end
  ```

  Ưu tiên dùng `next` trong vòng lặp thay vì khối điều kiện.

  ```Ruby
  # bad
  [0, 1, 2, 3].each do |item|
    if item > 1
      puts item
    end
  end

  # good
  [0, 1, 2, 3].each do |item|
    next unless item > 1
    puts item
  end
  ```

* <a name="map-find-select-reduce-size"></a>
  Ưu tiên dùng `map` hơn `collect`, `find` hơn `detect`, `select` hơn `find_all`,
  `reduce` hơn `inject` và `size` hơn `length`.
<sup>[[link](#map-find-select-reduce-size)]</sup>

* <a name="count-vs-size"></a>
  Đừng dùng `count` để thay cho `size`. Với đối tượng `Enumerable` hay là `Array`,
  nó sẽ duyệt qua từng phần tử để đếm số lượng phần tử, sẽ rất tốn thời gian.
<sup>[[link](#count-vs-size)]</sup>

  ```Ruby
  # bad
  some_hash.count

  # good
  some_hash.size
  ```

* <a name="flat-map"></a>
  Dùng `flat_map` thay cho `map` + `flatten`. Cái này không áp dụng cho mảng
  sâu hơn 2 cấp, vd: nếu `users.first.songs == ['a', ['b','c']]`,
  thì nên dùng `map + flatten` hơn là `flat_map`.
  `flat_map`  làm giảm đi 1 cấp, trong khi `flatten` làm phẳng hết về còn 1 cấp thôi.
<sup>[[link](#flat-map)]</sup>

  ```Ruby
  # bad
  all_songs = users.map(&:songs).flatten.uniq

  # good
  all_songs = users.flat_map(&:songs).uniq
  ```

* <a name="reverse-each"></a>
  Ưu tiên dùng `reverse_each` hơn `reverse.each` vì một số lớp mà `include
  Enumerable` sẽ hoạt động hiệu quả hơn.
<sup>[[link](#reverse-each)]</sup>

  ```Ruby
  # bad
  array.reverse.each { ... }

  # good
  array.reverse_each { ... }
  ```

## Cách đặt tên / Naming

> Khó khăn duy nhất trong lập trình là việc lưu giữ thông tin không hợp lệ
> và việc đặt tên.<br>
> -- Phil Karlton

* <a name="english-identifiers"></a>
  Dùng tiếng Anh để đặt tên.
<sup>[[link](#english-identifiers)]</sup>

  ```Ruby
  # bad - Dùng các ký tự không thuộc bảng mã ascii
  заплата = 1_000

  # bad - tên này là tiếng Bulgarian
  zaplata = 1_000

  # good
  salary = 1_000
  ```

* <a name="snake-case-symbols-methods-vars"></a>
  Dùng `snake_case` cho tên phương thức, biến và nhãn (symbol).
<sup>[[link](#snake-case-symbols-methods-vars)]</sup>

  ```Ruby
  # bad
  :'some symbol'
  :SomeSymbol
  :someSymbol

  someVar = 5

  def someMethod
    # some code
  end

  def SomeMethod
    # some code
  end

  # good
  :some_symbol

  def some_method
    # some code
  end
  ```

* <a name="camelcase-classes"></a>
  Dùng `CamelCase` cho tên lớp và module. (Giữ những từ viết tắt như HTTP, RFC,
  XML viết hoa)
<sup>[[link](#camelcase-classes)]</sup>

  ```Ruby
  # bad
  class Someclass
    # some code
  end

  class Some_Class
    # some code
  end

  class SomeXml
    # some code
  end

  class XmlSomething
    # some code
  end

  # good
  class SomeClass
    # some code
  end

  class SomeXML
    # some code
  end

  class XMLSomething
    # some code
  end
  ```

* <a name="snake-case-files"></a>
  Dùng `snake_case` cho tên file, vd: `hello_world.rb`.
<sup>[[link](#snake-case-files)]</sup>

* <a name="snake-case-dirs"></a>
  Dùng `snake_case` cho tên thư mục, vd:
  `lib/hello_world/hello_world.rb`.
<sup>[[link](#snake-case-dirs)]</sup>

* <a name="one-class-per-file"></a>
  Mỗi file chỉ nên có một lớp/module. Tên của file chính là tên của class/module
  nhưng thay PascalCase thành snake_case.
<sup>[[link](#one-class-per-file)]</sup>

* <a name="screaming-snake-case"></a>
  Dùng `SCREAMING_SNAKE_CASE` cho hằng số.
<sup>[[link](#screaming-snake-case)]</sup>

  ```Ruby
  # bad
  SomeConst = 5

  # good
  SOME_CONST = 5
  ```

* <a name="bool-methods-qmark"></a>
  Phương thức trả về boolean thì nên có thêm `?` đằng sau (vd: `Array#empty?`).
  Phương thức không trả về boolean thì không dùng.
<sup>[[link](#bool-methods-qmark)]</sup>

* <a name="bool-methods-prefix"></a>
  Tránh đặt tiền tố cho tên phương thức với các động từ bổ trợ như `is`,
  `does`, hay `can`. Những từ này không cần thiết và nghĩa quá chung chung,
  không đồng nhất với các phương thức boolean của ngôn ngữ: `empty?` hay `include?`.
<sup>[[link](#bool-methods-prefix)]</sup>

  ```Ruby
  # bad
  class Person
    def is_tall?
      true
    end

    def can_play_basketball?
      false
    end

    def does_like_candy?
      true
    end
  end

  # good
  class Person
    def tall?
      true
    end

    def basketball_player?
      false
    end

    def likes_candy?
      true
    end
  end
  ```

* <a name="dangerous-method-bang"></a>
  Tên của những phương thức có thể là *nguy hiểm* (vd: phương thức mà chỉnh sửa
  `self` hay các thuộc tính, hay phương thức `exit!` (không thực hiện
  việc `finalize` như `exit`), vv) cần kết thúc bằng một dấu chấm than `!` nếu như
  đã có một phiên bản an toàn của nó rồi (safe version).
<sup>[[link](#dangerous-method-bang)]</sup>

  ```Ruby
  # bad - there is no matching 'safe' method
  class Person
    def update!
    end
  end

  # good
  class Person
    def update
    end
  end

  # good
  class Person
    def update!
    end

    def update
    end
  end
  ```

* <a name="safe-because-unsafe"></a>
  Nếu có phương thức *nguy hiểm* thì nên có thêm phương thức an toàn.
<sup>[[link](#safe-because-unsafe)]</sup>

  ```Ruby
  class Array
    def flatten_once!
      res = []

      each do |e|
        [*e].each { |f| res << f }
      end

      replace(res)
    end

    def flatten_once
      dup.flatten_once!
    end
  end
  ```

* <a name="reduce-blocks"></a>
  Khi dùng `reduce` với block ngắn, đặt tên các tham số là `|a, e|`
  (accumulator, element).
<sup>[[link](#reduce-blocks)]</sup>

* <a name="other-arg"></a>

  Khi định nghĩa binary operators, đặt tên tham số là `other`
  (ngoại trừ hai toán tử `<<` và `[]` là ngoại lệ, bởi nó mang ý nghĩa khác).
<sup>[[link](#other-arg)]</sup>

  ```Ruby
  def +(other)
    # body omitted
  end
  ```

## Viết chú thích / Comments

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br>
> -- Steve McConnell

* <a name="no-comments"></a>
  Write self-documenting code and ignore the rest of this section. Seriously!
<sup>[[link](#no-comments)]</sup>

* <a name="english-comments"></a>
  Write comments in English.
<sup>[[link](#english-comments)]</sup>

* <a name="hash-space"></a>
  Use one space between the leading `#` character of the comment and the text
  of the comment.
<sup>[[link](#hash-space)]</sup>

* <a name="english-syntax"></a>
  Comments longer than a word are capitalized and use punctuation. Use [one
  space](https://en.wikipedia.org/wiki/Sentence_spacing) after periods.
<sup>[[link](#english-syntax)]</sup>

* <a name="no-superfluous-comments"></a>
  Avoid superfluous comments.
<sup>[[link](#no-superfluous-comments)]</sup>

  ```Ruby
  # bad
  counter += 1 # Increments counter by one.
  ```

* <a name="comment-upkeep"></a>
  Keep existing comments up-to-date. An outdated comment is worse than no
  comment at all.
<sup>[[link](#comment-upkeep)]</sup>

> Good code is like a good joke - it needs no explanation. <br>
> -- Russ Olsen

* <a name="refactor-dont-comment"></a>
  Avoid writing comments to explain bad code. Refactor the code to make it
  self-explanatory. (Do or do not - there is no try. --Yoda)
<sup>[[link](#refactor-dont-comment)]</sup>

### Comment Annotations

* <a name="annotate-above"></a>
  Annotations should usually be written on the line immediately above the
  relevant code.
<sup>[[link](#annotate-above)]</sup>

* <a name="annotate-keywords"></a>
  The annotation keyword is followed by a colon and a space, then a note
  describing the problem.
<sup>[[link](#annotate-keywords)]</sup>

* <a name="indent-annotations"></a>
  If multiple lines are required to describe the problem, subsequent lines
  should be indented three spaces after the `#` (one general plus two for
  indentation purpose).
<sup>[[link](#indent-annotations)]</sup>

  ```Ruby
  def bar
    # FIXME: This has crashed occasionally since v3.2.1. It may
    #   be related to the BarBazUtil upgrade.
    baz(:quux)
  end
  ```

* <a name="rare-eol-annotations"></a>
  In cases where the problem is so obvious that any documentation would be
  redundant, annotations may be left at the end of the offending line with no
  note. This usage should be the exception and not the rule.
<sup>[[link](#rare-eol-annotations)]</sup>

  ```Ruby
  def bar
    sleep 100 # OPTIMIZE
  end
  ```

* <a name="todo"></a>
  Use `TODO` to note missing features or functionality that should be added at
  a later date.
<sup>[[link](#todo)]</sup>

* <a name="fixme"></a>
  Use `FIXME` to note broken code that needs to be fixed.
<sup>[[link](#fixme)]</sup>

* <a name="optimize"></a>
  Use `OPTIMIZE` to note slow or inefficient code that may cause performance
  problems.
<sup>[[link](#optimize)]</sup>

* <a name="hack"></a>
  Use `HACK` to note code smells where questionable coding practices were used
  and should be refactored away.
<sup>[[link](#hack)]</sup>

* <a name="review"></a>
  Use `REVIEW` to note anything that should be looked at to confirm it is
  working as intended. For example: `REVIEW: Are we sure this is how the client
  does X currently?`
<sup>[[link](#review)]</sup>

* <a name="document-annotations"></a>
  Use other custom annotation keywords if it feels appropriate, but be sure to
  document them in your project's `README` or similar.
<sup>[[link](#document-annotations)]</sup>

## Lớp đối tượng và Module / Classes & Modules

* <a name="consistent-classes"></a>
  Use a consistent structure in your class definitions.
<sup>[[link](#consistent-classes)]</sup>

  ```Ruby
  class Person
    # extend and include go first
    extend SomeModule
    include AnotherModule

    # inner classes
    CustomErrorKlass = Class.new(StandardError)

    # constants are next
    SOME_CONSTANT = 20

    # afterwards we have attribute macros
    attr_reader :name

    # followed by other macros (if any)
    validates :name

    # public class methods are next in line
    def self.some_method
    end

    # initialization goes between class methods and other instance methods
    def initialize
    end

    # followed by other public instance methods
    def some_method
    end

    # protected and private methods are grouped near the end
    protected

    def some_protected_method
    end

    private

    def some_private_method
    end
  end
  ```

* <a name="file-classes"></a>
  Don't nest multi-line classes within classes. Try to have such nested
  classes each in their own file in a folder named like the containing class.
<sup>[[link](#file-classes)]</sup>

  ```Ruby
  # bad

  # foo.rb
  class Foo
    class Bar
      # 30 methods inside
    end

    class Car
      # 20 methods inside
    end

    # 30 methods inside
  end

  # good

  # foo.rb
  class Foo
    # 30 methods inside
  end

  # foo/bar.rb
  class Foo
    class Bar
      # 30 methods inside
    end
  end

  # foo/car.rb
  class Foo
    class Car
      # 20 methods inside
    end
  end
  ```

* <a name="modules-vs-classes"></a>
  Prefer modules to classes with only class methods. Classes should be used
  only when it makes sense to create instances out of them.
<sup>[[link](#modules-vs-classes)]</sup>

  ```Ruby
  # bad
  class SomeClass
    def self.some_method
      # body omitted
    end

    def self.some_other_method
      # body omitted
    end
  end

  # good
  module SomeModule
    module_function

    def some_method
      # body omitted
    end

    def some_other_method
      # body omitted
    end
  end
  ```

* <a name="module-function"></a>
  Favor the use of `module_function` over `extend self` when you want to turn
  a module's instance methods into class methods.
<sup>[[link](#module-function)]</sup>

  ```Ruby
  # bad
  module Utilities
    extend self

    def parse_something(string)
      # do stuff here
    end

    def other_utility_method(number, string)
      # do some more stuff
    end
  end

  # good
  module Utilities
    module_function

    def parse_something(string)
      # do stuff here
    end

    def other_utility_method(number, string)
      # do some more stuff
    end
  end
  ```

* <a name="liskov"></a>
  When designing class hierarchies make sure that they conform to the [Liskov
  Substitution
  Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle).
<sup>[[link](#liskov)]</sup>

* <a name="solid-design"></a>
  Try to make your classes as
  [SOLID](https://en.wikipedia.org/wiki/SOLID_\(object-oriented_design\)) as
  possible.
<sup>[[link](#solid-design)]</sup>

* <a name="define-to-s"></a>
  Always supply a proper `to_s` method for classes that represent domain
  objects.
<sup>[[link](#define-to-s)]</sup>

  ```Ruby
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    def to_s
      "#{@first_name} #{@last_name}"
    end
  end
  ```

* <a name="attr_family"></a>
  Use the `attr` family of functions to define trivial accessors or mutators.
<sup>[[link](#attr_family)]</sup>

  ```Ruby
  # bad
  class Person
    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    def first_name
      @first_name
    end

    def last_name
      @last_name
    end
  end

  # good
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end
  end
  ```

* <a name="attr"></a>
  Avoid the use of `attr`. Use `attr_reader` and `attr_accessor` instead.
<sup>[[link](#attr)]</sup>

  ```Ruby
  # bad - creates a single attribute accessor (deprecated in Ruby 1.9)
  attr :something, true
  attr :one, :two, :three # behaves as attr_reader

  # good
  attr_accessor :something
  attr_reader :one, :two, :three
  ```

* <a name="struct-new"></a>
  Consider using `Struct.new`, which defines the trivial accessors,
  constructor and comparison operators for you.
<sup>[[link](#struct-new)]</sup>

  ```Ruby
  # good
  class Person
    attr_accessor :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end
  end

  # better
  Person = Struct.new(:first_name, :last_name) do
  end
  ```

* <a name="no-extend-struct-new"></a>
  Don't extend an instance initialized by `Struct.new`. Extending it introduces
  a superfluous class level and may also introduce weird errors if the file is
  required multiple times.
<sup>[[link](#no-extend-struct-new)]</sup>

  ```Ruby
  # bad
  class Person < Struct.new(:first_name, :last_name)
  end

  # good
  Person = Struct.new(:first_name, :last_name)
  ```

* <a name="factory-methods"></a>
  Consider adding factory methods to provide additional sensible ways to
  create instances of a particular class.
<sup>[[link](#factory-methods)]</sup>

  ```Ruby
  class Person
    def self.create(options_hash)
      # body omitted
    end
  end
  ```

* <a name="duck-typing"></a>
  Prefer [duck-typing](https://en.wikipedia.org/wiki/Duck_typing) over
  inheritance.
<sup>[[link](#duck-typing)]</sup>

  ```Ruby
  # bad
  class Animal
    # abstract method
    def speak
    end
  end

  # extend superclass
  class Duck < Animal
    def speak
      puts 'Quack! Quack'
    end
  end

  # extend superclass
  class Dog < Animal
    def speak
      puts 'Bau! Bau!'
    end
  end

  # good
  class Duck
    def speak
      puts 'Quack! Quack'
    end
  end

  class Dog
    def speak
      puts 'Bau! Bau!'
    end
  end
  ```

* <a name="no-class-vars"></a>
  Avoid the usage of class (`@@`) variables due to their "nasty" behavior in
  inheritance.
<sup>[[link](#no-class-vars)]</sup>

  ```Ruby
  class Parent
    @@class_var = 'parent'

    def self.print_class_var
      puts @@class_var
    end
  end

  class Child < Parent
    @@class_var = 'child'
  end

  Parent.print_class_var # => will print 'child'
  ```

  As you can see all the classes in a class hierarchy actually share one
  class variable. Class instance variables should usually be preferred
  over class variables.

* <a name="visibility"></a>
  Assign proper visibility levels to methods (`private`, `protected`) in
  accordance with their intended usage. Don't go off leaving everything `public`
  (which is the default). After all we're coding in *Ruby* now, not in *Python*.
<sup>[[link](#visibility)]</sup>

* <a name="indent-public-private-protected"></a>
  Indent the `public`, `protected`, and `private` methods as much as the method
  definitions they apply to. Leave one blank line above the visibility modifier
  and one blank line below in order to emphasize that it applies to all methods
  below it.
<sup>[[link](#indent-public-private-protected)]</sup>

  ```Ruby
  class SomeClass
    def public_method
      # some code
    end

    private

    def private_method
      # some code
    end

    def another_private_method
      # some code
    end
  end
  ```

* <a name="def-self-class-methods"></a>
  Use `def self.method` to define class methods. This makes the code
  easier to refactor since the class name is not repeated.
<sup>[[link](#def-self-class-methods)]</sup>

  ```Ruby
  class TestClass
    # bad
    def TestClass.some_method
      # body omitted
    end

    # good
    def self.some_other_method
      # body omitted
    end

    # Also possible and convenient when you
    # have to define many class methods.
    class << self
      def first_method
        # body omitted
      end

      def second_method_etc
        # body omitted
      end
    end
  end
  ```

* <a name="alias-method-lexically"></a>
  Prefer `alias` when aliasing methods in lexical class scope as the
  resolution of `self` in this context is also lexical, and it communicates
  clearly to the user that the indirection of your alias will not be altered
  at runtime or by any subclass unless made explicit.
<sup>[[link](#alias-method-lexically)]</sup>

  ```Ruby
  class Westerner
    def first_name
      @names.first
    end

    alias given_name first_name
  end
  ```

  Since `alias`, like `def`, is a keyword, prefer bareword arguments over
  symbols or strings. In other words, do `alias foo bar`, not
  `alias :foo :bar`.

  Also be aware of how Ruby handles aliases and inheritance: an alias
  references the method that was resolved at the time the alias was defined;
  it is not dispatched dynamically.

  ```Ruby
  class Fugitive < Westerner
    def first_name
      'Nobody'
    end
  end
  ```

  In this example, `Fugitive#given_name` would still call the original
  `Westerner#first_name` method, not `Fugitive#first_name`. To override the
  behavior of `Fugitive#given_name` as well, you'd have to redefine it in the
  derived class.

  ```Ruby
  class Fugitive < Westerner
    def first_name
      'Nobody'
    end

    alias given_name first_name
  end
  ```

* <a name="alias-method"></a>
  Always use `alias_method` when aliasing methods of modules, classes, or
  singleton classes at runtime, as the lexical scope of `alias` leads to
  unpredictability in these cases.
<sup>[[link](#alias-method)]</sup>

  ```Ruby
  module Mononymous
    def self.included(other)
      other.class_eval { alias_method :full_name, :given_name }
    end
  end

  class Sting < Westerner
    include Mononymous
  end
  ```

## Exceptions

* <a name="prefer-raise-over-fail"></a>
  Ưu tiên dùng `raise` hơn `fail`.
  <sup>[[link](#prefer-raise-over-fail)]</sup>

  ```Ruby
  # bad
  fail SomeException, 'message'

  # good
  raise SomeException, 'message'
  ```

* <a name="no-explicit-runtimeerror"></a>
  Không cần phải chỉ rõ `RuntimeError` khi `raise` mà có hai đối số.
<sup>[[link](#no-explicit-runtimeerror)]</sup>

  ```Ruby
  # bad
  raise RuntimeError, 'message'

  # good - RuntimeError được raise mặc định
  raise 'message'
  ```

* <a name="exception-class-messages"></a>
  Khi ném exception, ưu tiên dùng phương án hai tham số: một class và một
  message hơn là một claas được khởi tạo với message.
<sup>[[link](#exception-class-messages)]</sup>

  ```Ruby
  # bad
  raise SomeException.new('message')
  # Lưu ý rằng lệnh sau `raise SomeException.new('message'), backtrace` là sai.

  # good
  raise SomeException, 'message'
  # Tương đương với `raise SomeException, 'message', backtrace`.
  ```

* <a name="no-return-ensure"></a>
  KHÔNG return từ `ensure` block. Nếu làm vậy, dù có exception được ném ra nhưng
  phương thức vẫn trả kết quả về, nên đôi khi ta sẽ bỏ qua exception.
<sup>[[link](#no-return-ensure)]</sup>

  ```Ruby
  # bad
  def foo
    raise
  ensure
    return 'very bad idea'
  end
  ```

* <a name="begin-implicit"></a>
  Dùng từ khóa `begin` một cách ngầm định khi có thể.
<sup>[[link](#begin-implicit)]</sup>

  ```Ruby
  # bad
  def foo
    begin
      # main logic goes here
    rescue
      # failure handling goes here
    end
  end

  # good
  def foo
    # main logic goes here
  rescue
    # failure handling goes here
  end
  ```

* <a name="contingency-methods"></a>
  Mitigate the proliferation of `begin` blocks by using *contingency methods*
  (a term coined by Avdi Grimm).
<sup>[[link](#contingency-methods)]</sup>

  ```Ruby
  # bad
  begin
    something_that_might_fail
  rescue IOError
    # handle IOError
  end

  begin
    something_else_that_might_fail
  rescue IOError
    # handle IOError
  end

  # good
  def with_io_error_handling
     yield
  rescue IOError
    # handle IOError
  end

  with_io_error_handling { something_that_might_fail }

  with_io_error_handling { something_else_that_might_fail }
  ```

* <a name="dont-hide-exceptions"></a>
  Đừng bỏ qua exceptions.
<sup>[[link](#dont-hide-exceptions)]</sup>

  ```Ruby
  # bad
  begin
    # an exception occurs here
  rescue SomeError
    # the rescue clause does absolutely nothing
  end

  # bad
  do_something rescue nil
  ```

* <a name="no-rescue-modifiers"></a>
  Bắt đúng lỗi được ném ra.
<sup>[[link](#no-rescue-modifiers)]</sup>

  ```Ruby
  # bad - this catches exceptions of StandardError class and its descendant classes
  read_file rescue handle_error($!)

  # good - this catches only the exceptions of Errno::ENOENT class and its descendant classes
  def foo
    read_file
  rescue Errno::ENOENT => ex
    handle_error(ex)
  end
  ```

* <a name="no-exceptional-flows"></a>
  Không dùng exceptions trong luông điều khiển.
  Thay vào đó hãy kiểm tra hợ lệ trước.
<sup>[[link](#no-exceptional-flows)]</sup>

  ```Ruby
  # bad
  begin
    n / d
  rescue ZeroDivisionError
    puts 'Cannot divide by 0!'
  end

  # good
  if d.zero?
    puts 'Cannot divide by 0!'
  else
    n / d
  end
  ```

* <a name="no-blind-rescues"></a>
  Tránh việc bắt lớp `Exception`. Việc này sẽ gọi đến hàm `exit`,
  có thể bạn sẽ phải tắt cứng process bằng lệnh `kill -9`.
<sup>[[link](#no-blind-rescues)]</sup>

  ```Ruby
  # bad
  begin
    # calls to exit and kill signals will be caught (except kill -9)
    exit
  rescue Exception
    puts "you didn't really want to exit, right?"
    # exception handling
  end

  # good
  begin
    # a blind rescue rescues from StandardError, not Exception as many
    # programmers assume.
  rescue => e
    # exception handling
  end

  # also good
  begin
    # an exception occurs here
  rescue StandardError => e
    # exception handling
  end
  ```

* <a name="exception-ordering"></a>
  Bắt exception theo thứ tự từ thấp đến cao, bắt thằng con trước, rồi bắt cha sau.
<sup>[[link](#exception-ordering)]</sup>

  ```Ruby
  # bad
  begin
    # some code
  rescue StandardError => e
    # some handling
  rescue IOError => e
    # some handling that will never be executed
  end

  # good
  begin
    # some code
  rescue IOError => e
    # some handling
  rescue StandardError => e
    # some handling
  end
  ```

* <a name="release-resources"></a>
  Trong `ensure`, nên giải phóng tài nguyên cho hệ thống.
<sup>[[link](#release-resources)]</sup>

  ```Ruby
  f = File.open('testfile')
  begin
    # .. process
  rescue
    # .. handle error
  ensure
    f.close if f
  end
  ```

* <a name="auto-release-resources"></a>
Nếu như tài nguyên được giải phóng tự động thì không cần phải làm thủ công.
<sup>[[link](#auto-release-resources)]</sup>

  ```Ruby
  # bad - bạn muốn đóng file một cách tường minh qua lệnh `close`
  f = File.open('testfile')
  # some action on the file
  f.close

  # good - file descriptor được đóng tự động khi dùng xong
  File.open('testfile') do |f|
    # some action on the file
  end
  ```

* <a name="standard-exceptions"></a>
  Ưu tiên dùng những exception mặc định của hệ thống hơn là định nghĩa là lớp mới.
<sup>[[link](#standard-exceptions)]</sup>

## Collections

* <a name="literal-array-hash"></a>
  Ưu tiên khai báo mảng hay hash bằng cặp ngoặc hơn là tạo thể hiện, bởi vì
  đôi khi ta phải truyền đối số.
<sup>[[link](#literal-array-hash)]</sup>

  ```Ruby
  # bad
  arr = Array.new
  hash = Hash.new

  # good
  arr = []
  hash = {}
  ```

* <a name="percent-w"></a>
  Ưu tiên dùng `%W` để tạo mảng. Lưu ý chỉ dùng khi mảng có vài phần tử.
<sup>[[link](#percent-w)]</sup>

  ```Ruby
  # bad
  STATES = ['draft', 'open', 'closed']

  # good
  STATES = %w(draft open closed)
  ```

* <a name="percent-i"></a>
  Ưu tiên dùng `%i` nếu muốn tạo mảng các nhãn (symbol).
<sup>[[link](#percent-i)]</sup>

  ```Ruby
  # bad
  STATES = [:draft, :open, :closed]

  # good
  STATES = %i(draft open closed)
  ```

* <a name="no-trailing-array-commas"></a>
  Tránh thêm dấu phẩy sau phẩn tử cuối cùng của `Array` hay `Hash`, đặc biệt là
  khi các phần tử đó không nằm trên các dòng khác nhau.
<sup>[[link](#no-trailing-array-commas)]</sup>

  ```Ruby
  # bad - easier to move/add/remove items, but still not preferred
  VALUES = [
             1001,
             2020,
             3333,
           ]

  # bad
  VALUES = [1001, 2020, 3333, ]

  # good
  VALUES = [1001, 2020, 3333]
  ```

* <a name="no-gappy-arrays"></a>
  Tránh việc tạo mảng nhiều phần tử quá mà không dùng hết.
<sup>[[link](#no-gappy-arrays)]</sup>

  ```Ruby
  arr = []
  arr[100] = 1 # now you have an array with lots of nils
  ```

* <a name="first-and-last"></a>
  Khi truy cập phần tử đầu tiên/cuối cùng trong mảng, ưu tiên dùng
  `first` hay `last` hơn `[0]`, `[-1]`.
<sup>[[link](#first-and-last)]</sup>

* <a name="set-vs-array"></a>
  Dùng `Set` thay vì `Array` khi làm việc với mảng các phần tử độc nhất.
  `Set` là tập hợp các phần tử không có thứ tự và không lặp lại. Đây là một
  biến thể của `Array`, nó là sự kết hợp các tính năng của `Array` và khả năng
  tìm kiếm nhanh của `Hash`.
<sup>[[link](#set-vs-array)]</sup>

* <a name="symbols-as-keys"></a>
  Khi làm việc với hash, ưu tiên dùng nhãn hơn string khi đặt key.
<sup>[[link](#symbols-as-keys)]</sup>

  ```Ruby
  # bad
  hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

  # good
  hash = { one: 1, two: 2, three: 3 }
  ```

* <a name="no-mutable-keys"></a>
  Tránh việc sử dụng đối tượng không bất biến cho hash key.
<sup>[[link](#no-mutable-keys)]</sup>

* <a name="hash-literals"></a>
  Dùng cú pháp của Ruby 1.9 để tạo hash.
<sup>[[link](#hash-literals)]</sup>

  ```Ruby
  # bad
  hash = { :one => 1, :two => 2, :three => 3 }

  # good
  hash = { one: 1, two: 2, three: 3 }
  ```

* <a name="no-mixed-hash-syntaces"></a>
  Không trộn cú pháp của Ruby 1.9 với kiểu cũ.
<sup>[[link](#no-mixed-hash-syntaces)]</sup>

  ```Ruby
  # bad
  { a: 1, 'b' => 2 }

  # good
  { :a => 1, 'b' => 2 }
  ```

* <a name="hash-key"></a>
  Dùng `Hash#key?` thay cho `Hash#has_key?`, `Hash#value?` thay cho
  `Hash#has_value?`. Xem thêm:
  [here](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/43765)
<sup>[[link](#hash-key)]</sup>

  ```Ruby
  # bad
  hash.has_key?(:test)
  hash.has_value?(value)

  # good
  hash.key?(:test)
  hash.value?(value)
  ```

* <a name="hash-each"></a>
  Dùng `Hash#each_key` thay cho `Hash#keys.each`, và `Hash#each_value`
  thay cho `Hash#values.each`.
<sup>[[link](#hash-each)]</sup>

  ```Ruby
  # bad
  hash.keys.each { |k| p k }
  hash.values.each { |v| p v }
  hash.each { |k, _v| p k }
  hash.each { |_k, v| p v }

  # good
  hash.each_key { |k| p k }
  hash.each_value { |v| p v }
  ```

* <a name="hash-fetch"></a>
  Dùng `Hash#fetch` khi biết chắc rằng hash có key đó hay không.
<sup>[[link](#hash-fetch)]</sup>

  ```Ruby
  heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
  # bad - if we make a mistake we might not spot it right away
  heroes[:batman] # => 'Bruce Wayne'
  heroes[:supermann] # => nil

  # good - fetch raises a KeyError making the problem obvious
  heroes.fetch(:supermann)
  ```

* <a name="hash-fetch-defaults"></a>
  Khi dùng `Hash#fetch`, có thể thêm giá trị mặc định vào thay vì dùng logic.
<sup>[[link](#hash-fetch-defaults)]</sup>

  ```Ruby
  batman = { name: 'Bruce Wayne', is_evil: false }

  # bad - if we just use || operator with falsy value we won't get the expected result
  batman[:is_evil] || true # => true

  # good - fetch work correctly with falsy values
  batman.fetch(:is_evil, true) # => false
  ```

* <a name="use-hash-blocks"></a>
  Ưu tiên dùng block thay vì giá trị mặc định trong `Hash#fetch`,
  vì có thể sẽ làm chậm chương trình đi, hoặc có những tác dụng phụ.
  <sup>[[link](#use-hash-blocks)]</sup>

  ```Ruby
  batman = { name: 'Bruce Wayne' }

  # bad - if we use the default value, we eager evaluate it
  # so it can slow the program down if done multiple times
  batman.fetch(:powers, obtain_batman_powers) # obtain_batman_powers is an expensive call

  # good - blocks are lazy evaluated, so only triggered in case of KeyError exception
  batman.fetch(:powers) { obtain_batman_powers }
  ```

* <a name="hash-values-at"></a>
  Dùng `Hash#values_at` khi muốn lấy nhiều giá trị liên tiếp trong hash ra.
<sup>[[link](#hash-values-at)]</sup>

  ```Ruby
  # bad
  email = data['email']
  username = data['nickname']

  # good
  email, username = data.values_at('email', 'nickname')
  ```

* <a name="ordered-hashes"></a>
  Lưu ý rằng hash trừ Ruby 1.9 là có thứ tự.
<sup>[[link](#ordered-hashes)]</sup>

* <a name="no-modifying-collections"></a>
  KHÔNG chỉnh sửa một collection khi đang duyệt qua các phần tử của nó.
<sup>[[link](#no-modifying-collections)]</sup>

* <a name="accessing-elements-directly"></a>
  Khi truy cập vào các phần tử của collection, tránh việc truy cập trực tiếp
  thông qua `[n]`, hãy dùng các phương án thay thế nếu có thể.
  Việc này sẽ giúp tránh được lỗi gọi `[]` on `nil`
<sup>[[link](#accessing-elements-directly)]</sup>

  ```Ruby
  # bad
  Regexp.last_match[1]

  # good
  Regexp.last_match(1)
  ```

* <a name="provide-alternate-accessor-to-collections"></a>
  Khi cấp quyền truy cập vào một collection, nên cung cấp một phương án
  giúp users khỏi phải check `nil` sau khi nhận kết quả trả về.
<sup>[[link](#provide-alternate-accessor-to-collections)]</sup>

  ```Ruby
  # bad
  def awesome_things
    @awesome_things
  end

  # good
  def awesome_things(index = nil)
    if index && @awesome_things
      @awesome_things[index]
    else
      @awesome_things
    end
  end
  ```
## Numbers

* <a name="integer-type-checking"></a>
  Dùng `Integer` để kiểm tra kiểu của một số. `Fixnum` thì phụ thuộc vào nền tảng
  (hệ điều hành chẳng hạn), với hệ thống `32-bit` và `64-bit` thì kết quả nó
  sẽ khác nhau.
<sup>[[link](#integer-type-checking)]</sup>

  ```Ruby
  timestamp = Time.now.to_i

  # bad
  timestamp.is_a? Fixnum
  timestamp.is_a? Bignum

  # good
  timestamp.is_a? Integer
  ```

## Strings

* <a name="string-interpolation"></a>
  Ưu tiên dùng kiểu nhúng vào String hoặc là `format` hơn là nối chuỗi.
<sup>[[link](#string-interpolation)]</sup>

  ```Ruby
  # bad
  email_with_name = user.name + ' <' + user.email + '>'

  # good
  email_with_name = "#{user.name} <#{user.email}>"

  # good
  email_with_name = format('%s <%s>', user.name, user.email)
  ```

* <a name="consistent-string-literals"></a>
  Với String tuyệt đối, hiện có hai kiểu dùng: dấu nháy đơn (Option A)
  và dấu nháy kép (Option B).
<sup>[[link](#consistent-string-literals)]</sup>

  * **(Option A)**

    ```Ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

  * **(Option B)**

    ```Ruby
    # bad
    name = 'Bozhidar'

    # good
    name = "Bozhidar"
    ```

  Tài liệu này sử dụng kiểu thứ nhất.

* <a name="no-character-literals"></a>
  Không dùng cú pháp `?x` nữa, Ruby 1.9 đã bỏ rồi.
<sup>[[link](#no-character-literals)]</sup>

  ```Ruby
  # bad
  char = ?c

  # good
  char = 'c'
  ```

* <a name="curlies-interpolate"></a>
  Khi nhúng biến toàn cục vào String, không nên bỏ cặp ngoặc nhọn đi.
<sup>[[link](#curlies-interpolate)]</sup>

  ```Ruby
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    # bad - hợp lệ, nhưng khó đọc
    def to_s
      "#@first_name #@last_name"
    end

    # good
    def to_s
      "#{@first_name} #{@last_name}"
    end
  end

  $global = 0
  # bad
  puts "$global = #$global"

  # good
  puts "$global = #{$global}"
  ```

* <a name="no-to-s"></a>
  Không cần phải dùng `Object#to_s` khi in đối tượng ra, mặc định nó có rồi.
<sup>[[link](#no-to-s)]</sup>

  ```Ruby
  # bad
  message = "This is the #{result.to_s}."

  # good
  message = "This is the #{result}."
  ```

* <a name="concat-strings"></a>
  Tránh sử dụng `String#+` khi muốn tạo string với nhiều dữ liệu.
  Thay vào đó, hãy dùng `String#<<`. `String#+` sẽ tạo một đối tượng mới,
  do đó sẽ chậm hơn, `String#<<` ngược lại chỉ thêm vào.
<sup>[[link](#concat-strings)]</sup>

  ```Ruby
  # bad
  html = ''
  html += '<h1>Page title</h1>'

  paragraphs.each do |paragraph|
    html += "<p>#{paragraph}</p>"
  end

  # good and also fast
  html = ''
  html << '<h1>Page title</h1>'

  paragraphs.each do |paragraph|
    html << "<p>#{paragraph}</p>"
  end
  ```

* <a name="dont-abuse-gsub"></a>
  Trong một số trường hợp cụ thể cần tốc độ, không dùng `String#gsub`.
<sup>[[link](#dont-abuse-gsub)]</sup>

    ```Ruby
    url = 'http://example.com'
    str = 'lisp-case-rules'

    # bad
    url.gsub('http://', 'https://')
    str.gsub('-', '_')

    # good
    url.sub('http://', 'https://')
    str.tr('-', '_')
    ```

* <a name="heredocs"></a>
  Khi sử dụng heredocs cho string nhiều dòng, để ý rằng nó sẽ bỏ qua mấy
  khoảng trắng, cho nên tốt nhất nên làm một ký tự `|` ở đầu dòng để làm mốc.
<sup>[[link](#heredocs)]</sup>

  ```Ruby
  code = <<-END.gsub(/^\s+\|/, '')
    |def test
    |  some_method
    |  other_method
    |end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ```

* <a name="squiggly-heredocs"></a>
  Sử dụng cú pháp `~` của Ruby 2.3 để hiển thị tốt hơn.
<sup>[[link](#squiggly-heredocs)]</sup>

  ```Ruby
  # bad - using Powerpack String#strip_margin
  code = <<-END.strip_margin('|')
    |def test
    |  some_method
    |  other_method
    |end
  END

  # also bad
  code = <<-END
  def test
    some_method
    other_method
  end
  END

  # good
  code = <<~END
    def test
      some_method
      other_method
    end
  END
  ```

## Percent Literals

* <a name="percent-q-shorthand"></a>
  Dùng `%()` (hoặc `%Q`) cho string một-dòng với yêu cầu là
  nó phải vừa in ra được mà vừa nhúng được.
  Với string nhiều dòng, ưu tiên dùng heredocs.
<sup>[[link](#percent-q-shorthand)]</sup>

  ```Ruby
  # bad (no interpolation needed)
  %(<div class="text">Some text</div>)
  # should be '<div class="text">Some text</div>'

  # bad (no double-quotes)
  %(This is #{quality} style)
  # should be "This is #{quality} style"

  # bad (multiple lines)
  %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
  # should be a heredoc.

  # good (requires interpolation, has quotes, single line)
  %(<tr><td class="name">#{name}</td>)
  ```

* <a name="percent-q"></a>
  Tránh dùng `%q` trừ khi bạn có string mà chứa cả `'` và `"`.
  String bình thường vẫn dễ đọc hơn, trừ khi nó có chứa nhiều ký tự đặc biệt quá.
<sup>[[link](#percent-q)]</sup>

  ```Ruby
  # bad
  name = %q(Bruce Wayne)
  time = %q(8 o'clock)
  question = %q("What did you say?")

  # good
  name = 'Bruce Wayne'
  time = "8 o'clock"
  question = '"What did you say?"'
  quote = %q(<p class='quote'>"What did you say?"</p>)
  ```

* <a name="percent-r"></a>
  Chỉ dùng `%r` khi regex khớp *ít nhất* một ký tự `/`
<sup>[[link](#percent-r)]</sup>

  ```Ruby
  # bad
  %r{\s+}

  # good
  %r{^/(.*)$}
  %r{^/blog/2016/(.*)$}
  ```

* <a name="percent-x"></a>
  Tránh dùng `%x` trừ khi bạn muốn thực thi một command với dấu nháy ngược
  trong lệnh, mà thường hiếm khi gặp trường hợp này lắm.
<sup>[[link](#percent-x)]</sup>

  ```Ruby
  # bad
  date = %x(date)

  # good
  date = `date`
  echo = %x(echo `date`)
  ```

* <a name="percent-s"></a>
  Tránh dùng `%s`. Cộng đồng đã chọn `:"some string"` là cách để tạo symbol
  mà có khoảng trắng rồi.
<sup>[[link](#percent-s)]</sup>

* <a name="percent-literal-braces"></a>
  Ưu tiên `()` khi dùng với `%`, ngoại trừ `%r`.
<sup>[[link](#percent-literal-braces)]</sup>

  ```Ruby
  # bad
  %w[one two three]
  %q{"Test's king!", John said.}

  # good
  %w(one two three)
  %q("Test's king!", John said.)
  ```

  ```

## Các công cụ / Tools

### RuboCop

[RuboCop]

### RubyMine

[RubyMine](http://www.jetbrains.com/ruby/)
[partially based](http://confluence.jetbrains.com/display/RUBYDEV/RubyMine+Inspections)

