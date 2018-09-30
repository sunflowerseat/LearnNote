#Swift笔记3

##泛型

- protocol 抽象泛型

  ```Swift
  protocol OfflineParamModel: class {
      associatedtype T
  }
  
  
  class LocalSampleCreate : OfflineParamModel{
      typealias T = ProjectSample
  }
  ```

- Class 定义泛型

  ```Swift
  class OfflineParamModel<T : Any> {
  }
  ```

  