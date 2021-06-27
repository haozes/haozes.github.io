---
layout: post
title: SwiftUI ObservableObject 子属性修改不能刷新问题
date: 2021-06-27
categories: blog
tags: [Coding]
description: SwiftUI Nested ObservableObject property refresh problem
---
SwiftUI 以下代码中，在点击Button后，将child 值增加，但实际MainView显示未变化

```
class MainModel : ObservableObject {
    @Published var child : ChildModel
   
    init(element : ChildModel) {
        self.child = element

        
    }
}

class ChildModel : ObservableObject {
    @Published var value : Int
    init(value : Int) {
        self.value = value
    }
}

struct MainView: View {
    @ObservedObject var model : MainModel
   
    
    var body: some View {
        HStack {
            Text("Detail:")
            Text("\(model.child.value)")
            Button("Change Numbers"){
                model.child.value += 1
            }
        }
    }
}

struct ContentView_Previews: PreviewProvider {

    static var previews: some View {
        MainView(model: MainModel(element: ChildModel(value: 0)))
    }
}
```

## 方法一：将子对象改为struct
struct 是值类型

```
struct ChildModel {
    var value : Int
}
```
## 方法二：增加使用子控件

```
struct ChildView: View {
    @ObservedObject var element : ChildModel
    var body: some View {
      
        Text("\(element.value)")
    }
}

struct MainView: View {
    @ObservedObject var model : MainModel
    var body: some View {
        HStack {
            Text("Detail:")
            //Text("\(model.child.value)")
            ChildView(element: model.child)
            Button("Change Numbers"){
                model.child.value += 1
            }
        }
    }
}
```
## 方法三：使用sink 监听子对象的数据变化

```
class MainModel : ObservableObject {
    @Published var child : ChildModel
    var cancellable : AnyCancellable?
    init(element : ChildModel) {
        self.child = element
        self.cancellable = self.child.$value.sink(
                  receiveValue: { [weak self] _ in
                      self?.objectWillChange.send()
                  }
        )
        
    }
}
```
这种方法不够优雅，建议不要使用。

reference:  
- https://rhonabwy.com/2021/02/13/nested-observable-objects-in-swiftui/
- https://stackoverflow.com/questions/58406287/- how-to-tell-swiftui-views-to-bind-to-nested-observableobjects