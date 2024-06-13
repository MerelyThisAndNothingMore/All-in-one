---
tags:
  - AndroidJetPack
aliases:
---

StateFlow 是 [[Kotlin]] 的一个冷流（Cold [[Flow]]），它主要用于在 [[Android]] 应用中处理状态管理和状态更新。它是 [[LiveData]] 的一种替代方案，具有更强的灵活性和功能性。StateFlow 是一种特别设计用于状态持有和状态共享的 Flow，能够保证数据的最新状态，并在数据更新时通知所有的收集者。

# 示例

```kotlin
import androidx.lifecycle.ViewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow

class MyViewModel : ViewModel() {
    private val _uiState = MutableStateFlow("Initial State")
    val uiState: StateFlow<String> = _uiState

    fun updateState(newState: String) {
        _uiState.value = newState
    }
}

```

```kotlin
import android.os.Bundle
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.flow.collect
import kotlinx.coroutines.launch

class MyActivity : AppCompatActivity() {
    private val viewModel: MyViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // 收集 StateFlow 的状态更新
        lifecycleScope.launch {
            viewModel.uiState.collect { state ->
                // 更新UI
                findViewById<TextView>(R.id.textView).text = state
            }
        }

        // 更新状态
        findViewById<Button>(R.id.button).setOnClickListener {
            viewModel.updateState("New State")
        }
    }
}
```



