<div align="center">
<img src="https://i.suar.me/n9vGN/m" />

<h1>اسلوب الكود</h1>

</div>

<div dir="rtl">

هنا اسلوب كتابة الاكواد الذي يجب على كل مساهم اتباعه. من فضلك، اقرأ الوثيقة بأكملها قبل أن تبدأ في رفع الاكواد.

## Generics
دائمآ قم بكتابتها بـ `where`

#### الطريقة الخاطئة
<div dir="ltr">

```rust
 pub fn new<N: Into<String>,
               T: Into<String>,
               P: Into<InputFile>,
               E: Into<String>>
    (user_id: i32, name: N, title: T, png_sticker: P, emojis: E) -> Self { ... }
```

</div>

#### الطريقة الصحيحة
<div dir="ltr">

```rust
pub fn new<N, T, P, E>(user_id: i32, name: N, title: T, png_sticker: P, emojis: E) -> Self
    where
        N: Into<String>,
        T: Into<String>,
        P: Into<InputFile>,
        E: Into<String> { ... }
```

</div>

## التعليقات
التعليقات يجب ان تشرح مالذي يفعله الكود، وليس كيف يعمل الكود، وايضآ يجب عليك الاتزام بالاحرف الكبيرة في البداية وايضآ القواعد الاملائية ونقطة النهاية

#### الطريقة الخاطئة
<div dir="ltr">

```rust
/// this function make request to telegram
pub fn make_request(url: &str) -> String { ... }
```

</div>

#### الطريقة الصحيحة
<div dir="ltr">

```rust
/// This function makes a request to Telegram.
pub fn make_request(url: &str) -> String { ... }
```

</div>

وحاول اضافة الروابط قدر المستطاع:

<div dir="ltr">

```rust
/// Download a file from Telegram.
///
/// `path` can be obtained from the [`Bot::get_file`].
///
/// To download into [`AsyncWrite`] (e.g. [`tokio::fs::File`]), see
/// [`Bot::download_file`].
///
/// [`Bot::get_file`]: crate::bot::Bot::get_file
/// [`AsyncWrite`]: tokio::io::AsyncWrite
/// [`tokio::fs::File`]: tokio::fs::File
/// [`Bot::download_file`]: crate::Bot::download_file
#[cfg(feature = "unstable-stream")]
pub async fn download_file_stream(
    &self,
    path: &str,
) -> Result<impl Stream<Item = Result<Bytes, reqwest::Error>>, reqwest::Error>
{
    download_file_stream(&self.client, &self.token, path).await
}
```

</div>

## استخدم `Self` قدر المستطاع
#### الطريقة الخاطئة
<div dir="ltr">

```rust
impl ErrorKind {
    fn print(&self) {
        ErrorKind::Io => println!("Io"),
        ErrorKind::Network => println!("Network"),
        ErrorKind::Json => println!("Json"),
    }
}
```

</div>

#### الطريقة الصحيحة
<div dir="ltr">

```rust
impl ErrorKind {
    fn print(&self) {
        Self::Io => println!("Io"),
        Self::Network => println!("Network"),
        Self::Json => println!("Json"),
    }
}
```

</div>

<details>
    <summary>الميزد من الامثلة</summary>
    
#### الطريقة الخاطئة
<div dir="ltr">

```rust
impl<'a> AnswerCallbackQuery<'a> {
    pub(crate) fn new<C>(bot: &'a Bot, callback_query_id: C) -> AnswerCallbackQuery<'a>
    where
C: Into<String>, { ... }
```

</div>

#### الطريقة الصحيحة
<div dir="ltr">

```rust
impl<'a> AnswerCallbackQuery<'a> {
    pub(crate) fn new<C>(bot: &'a Bot, callback_query_id: C) -> Self
    where
C: Into<String>, { ... }
```

</div>
</details>


## التسمية
* ابتعد عن التكرار غير الضروري، مثال `Message::message_id` اجعلها `Message::id` ويمكنك استخدام `[serde(rename = "message_id")]#` اذ لزم.
* اما في الـ generic استخدم الاسم `S` للـ streams، واستخدم `Fut` للـ futures، واستخدم `F` للدوال ( قدر المستطاع ).

## عامة
* اضف للدالة `[must_use]#` اذا كانت ترجع قيمة *يجب* استخدامها.
* دائمآ اكتب `<log::<op` بدلآ من استدعاءها وكتابتها بهذه الطريقة `<op>`. مثلآ اكتب `log::info!("blah")`.


</div>

> source: [teloxide/CODE_STYLE.md](https://github.com/teloxide/teloxide/blob/master/CODE_STYLE.md)
