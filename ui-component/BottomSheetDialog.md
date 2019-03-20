## BottomSheetDialogFragment

### 让BottomSheetDialogFragment不能下拉消失的方法

1、重写`onCreateDialog`

```java
override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
        val dialog = super.onCreateDialog(savedInstanceState)
        if (recordViewModel.isRapMode()) {
            dialog.setCanceledOnTouchOutside(false)
            dialog.setCancelable(false)
            dialog.setOnKeyListener { dialog, keyCode, event ->
                if (keyCode == KeyEvent.KEYCODE_BACK) {
                    tryToDismissDialog()
                    true
                } else {

                    false
                }
            }
        }
        return dialog

    }
```

2、`onViewCreated` 中设置不允许cancel

```java
override fun onViewCreated(view: View?, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        isCancelable = false
}
```

---

### 拦截透明区域点击事件的方法

```java
override fun onViewCreated(view: View?, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
val ppp = view?.parent?.parent
            if (ppp is ViewGroup) {
                ppp.findViewById<View>(R.id.touch_outside)?.setOnClickListener {
                    //点击弹出透明区域
                    tryToDismissDialog()
                }
            }
}
```