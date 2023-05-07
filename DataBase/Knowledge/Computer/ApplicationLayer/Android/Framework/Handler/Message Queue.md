---
tags: 
alias:
---

内部存储结构并不是真正的队列，而是采用单链表的数据结构来存储消息列表 这点和传统的队列有点不一样， 主要区别在于Android的这个队列中的消息是按照时间先后顺序来存储的，时间较早的消息，越靠近队头。 当然，我 们也可以理解成，它是先进先出的，只是这里的先依据的不是谁先入队，而是消息待发送的时间
# Looper.quit、quitSafely的区别

当我们调用Looper的quit方法时，实际上执行了MessageQueue中的removeAllMessagesLocked方法，该方法 的作用是把MessageQueue消息池中所有的消息全部清空， 无论是延迟消息(延迟消息是指通过 sendMessageDelayed或通过postDelayed等方法发送的需要延迟执行的消息)还是非延迟消息。

当我们调用Looper的quitSafely方法时，实际上执行了MessageQueue中的removeAllFutureMessagesLocked 方法，通过名字就可以看出，该方法只会清空MessageQueue消息池中所有的延迟消息，并将消息池中所有的非延 迟消息派发出去让Handler去处理，quitSafely相比于quit方法安全之处在于清空消息之前会派发所有的非延迟消 息。


# 阻塞
调用 MessageQueue.next() 方法的时候会调用 Native 层的 nativePollOnce() 方法进行精准时间的阻塞。在 Native 层，将进入 pullInner() 方法，使用 epoll_wait 阻塞等待以读取管道的通知。如果没有从 Native 层得到消 息，那么这个方法就不会返回。此时主线程会释放 CPU 资源进入休眠状态。