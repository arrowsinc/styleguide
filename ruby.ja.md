# Ruby Coding Guide

## 参考
http://bojovs.com/2012/04/24/ruby-coding-style/
https://github.com/framgia/coding-standards/blob/master/ja/ruby/standard.md
https://github.com/bbatsov/ruby-style-guide
https://github.com/cookpad/styleguide/blob/master/ruby.ja.md#%E6%AD%A3%E8%A6%8F%E8%A1%A8%E7%8F%BE

## 基本
* インデントは2文字空白を使用する。タブは使用しない。
* 1行の文字数は基本80文字とする(全角文字は2文字)。長くなったとしても120文字を超えない。
* 式の途中で改行する場合は、2行目以降を1行目より1段深くインデントする。
* 行末に空白を置かない。
* ファイルの末尾に空行を置かない。

## スペースの入れ方

* カンマ、コロン、セミコロンの前にはスペースを入れず、後ろにスペースを入れる。
* 演算子の前後にはスペースを入れる。
* ```{}```の内側にはスペースを入れる。
* ```()```、```[]```の内側にはスペースを入れない。

```ruby
# good
e = M * c**2 # べき乗の演算子の前後にはスペースを入れない
[1, 2, 3].each { |e| puts e }
```

## 文字列
* 文字列の結合は使用せず、式展開を使用する```"#{str1}...#{str2}```。
* 1変数のみの式展開```"#{str}"```は使用しない。
* 式展開やバックスラッシュ記法( \t や \n など) を使用しない場合はシングルクォートを使用する。
* 大きな文字列を結合する場合は、String#+ を使用せず String#<< を使用する。

```ruby
# good
paragraphs.each do |paragraph|
  html << "<p>#{paragraph}</p>"
end
```

## 配列、ハッシュ

* 空配列、空ハッシュは```[]```、```{}```と書く。```Array.new```、```Hash.new``` は使わない。
* 文字列配列は%記法 ```%w(...)```を用いて書く。
* ハッシュはHash1.9形式 ```{ key: value }``` を使う。

```ruby
# good
{ hoge: 1, fuga: 2 }

# bad
{hoge: 1, fuga: 2}
```

## 条件式

* ```if !condition``` の代わりに ```unless condition``` を使う。
* ```while !condition``` の代わりに ```until condition``` を使う。
* ```unless``` に対して ```else``` を使ってはならない。
* ```unless``` および ```until``` の条件式に複数の項を ```||``` で結合した論理式を書いてはならない。
* if/then/else/end が1行で書けるような処理の場合は、代わりに三項演算子を使用する。
* 条件演算子を入れ子にしてはならない。
* 条件部には括弧を利用しない。


## caseメソッド
* ```case``` 式と ```when``` 節のインデントは同じ深さにします。 ただし、 case 式を変数に代入するときは when 節に合わせず、単純なインデントで記述します。

```ruby
# good
case
when song.name == 'Misty'
  puts 'Not again!'
when song.duration > 120
    puts 'Too long!'
else
  song.play
end

# good
kind = case year
  when 1850..1889 then 'Blues'
  when 1890..1909 then 'Ragtime'
  else 'Jazz'
  end

# bad
kind = case year
       when 1850..1889 then 'Blues'
       when 1890..1909 then 'Ragtime'
       else 'Jazz'
       end
```

## eachメソッド

* ```for``` 式は使用せず、```each``` メソッドを使用する。
* 1行で記述されるブロックには中括弧 ```{...}``` を使用する。
* その1行が複雑だったり横に長かったりする場合や、処理を複数行記述するときは、```do...end``` を使用する。

```ruby
# good
names.each { |name| puts name }

# good
names.each do |name|
  name = fukuzatsu.na.shori(name)
  name = sugoku.nagai.shori(name)
  puts name
end

# bad
names.each do |name|
  puts name
end

# bad
names.each { |name| puts sugoku.nagai.shori(fukuzatsu.na.shori(name)) }
```
