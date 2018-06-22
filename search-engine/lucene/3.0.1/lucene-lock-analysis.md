> lock,lockFactory这里用了工厂方法设计模式

#### lock的使用

    new Lock.With(directory.makeLock("my.lock"), lockWaitTimeout) 
    {
      public Object doBody() 
      {
        //code to execute while locked
      }
    }.run();

#### Lock Implementation

    1.NoLock
    2.SimpleFSLock 在目录下创建文件成功则获取锁成功;释放锁即删除文件
    3.SingleInstanceLock 获取锁时往HashSet<String>里添加,释放锁时从里删除
    
          public boolean obtain() throws IOException {
            synchronized(locks) {
              return locks.add(lockName);
            }
          }
    
    4.NativeFSLock 基于NIO里面的FileLock实现的排他锁
    
        [xxx]FileChannel.lock([xxx])/tryLock([xxx]);
        默认情况下是排他锁[共享锁,独占锁]
        一些不支持共享锁的操作系统,将自动将共享锁改成排它锁,可以通过调用isShared()方法来检测获得的是什么类型
        如果多个应用部署到同一台机器上,并且同时操作同一份数据(数据库中或文件中的数据),可以使用FileLock充当分布式锁
        ***更多查看api文档***
        这个FileLock有平台依赖性,使用前确保了解对应平台下的情况
    
    5.CheckedLock
    
        获取锁和释放锁前,通过socket发送数据给另一个进程并接收响应来判断是否创建锁和销毁锁
        never results in two processes holding the lock at the same time

#### LockFactory Implementation



#### FSDirectory.open调用

      public static FSDirectory open(File path) throws IOException 
      {
        return open(path, null);
      }
      public static FSDirectory open(File path, LockFactory lockFactory) throws IOException 
      {
        if (Constants.WINDOWS) {
          return new SimpleFSDirectory(path, lockFactory);
        } else {
          return new NIOFSDirectory(path, lockFactory);
        }
      }

---












