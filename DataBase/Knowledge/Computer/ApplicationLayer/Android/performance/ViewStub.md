---
tags: 
alias:
---

ViewStub是Android中的一种轻量级视图，它可以被用来延迟加载布局。ViewStub的原理是在布局中占据一个很小的空间，当需要加载时，会被替换为指定的布局。这种延迟加载的方式可以优化布局的性能，避免在页面启动时就加载所有的视图，从而提高应用程序的响应速度和用户体验。

在ViewStub中，以下方法会被跳过：

-   invalidate()
-   requestLayout()
-   setVisibility()

这是因为ViewStub并不是一个真正的视图，它只是在布局中占据了一个位置，并没有实际的UI元素。因此，对于这些方法的调用，ViewStub会直接忽略它们。

当需要加载ViewStub时，可以调用inflate()方法来替换ViewStub。inflate()方法会将ViewStub替换为指定的布局，并将其添加到父布局中。在替换完成后，ViewStub会被从父布局中移除，以释放占用的空间。

需要注意的是，一旦ViewStub被替换为实际的视图，就无法再次使用它来延迟加载其他布局。如果需要再次使用延迟加载功能，需要重新创建一个新的ViewStub对象。