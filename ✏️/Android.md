* 跳转回调

  ```kotlin
  val mResultLaunch = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {
    Loger.d("BankCardFragment_resultCode: ${it.resultCode}")
  }
  mResultLaunch.launch(Intent(context, MainActivity::class.java))
  ```

  



