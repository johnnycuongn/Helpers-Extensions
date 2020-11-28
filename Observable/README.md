# Observable

## Use

Data Binding

## Code

``` Swift
public final class Observable<Value> {
    
    struct Observer<Value> {
        weak var observer: AnyObject?
        let block: (Value) -> Void
    }
    
    private var observers = [Observer<Value>]()
    
    public var value: Value {
        didSet { notifyObservers() }
    }
    
    public init(_ value: Value) {
        self.value = value
    }
    
    public func observe(on observer: AnyObject, observerBlock: @escaping (Value) -> Void) {
        observers.append(Observer(observer: observer, block: observerBlock))
        observerBlock(self.value)
    }
    
    public func remove(observer: AnyObject) {
        observers = observers.filter { $0.observer !== observer }
    }
    
    private func notifyObservers() {
        for observer in observers {
            DispatchQueue.main.async { observer.block(self.value) }
        }
    }
}
```
## Example

```Swift 
class ViewModel {
    var item: Observable<Int> = Observable(100)
}

class ViewController {
    
    var viewModel: ViewModel
    
    init(viewModel: ViewModel) {
        self.viewModel = viewModel
        bind(to: self.viewModel)
    }
    
    private func bind(to viewmodel: ViewModel) {
        viewmodel.item.observe(on: self) { [weak self] in
            self?.updateTable($0)
        }
    }
    
    private func updateTable(_ value: Int) {
        print("Successfully Update Table with value: \(value)")
    }
}

let myViewModel = ViewModel()
let viewController = ViewController(viewModel: myViewModel)

// Change View Model Item
myViewModel.item.value = 50

// Logger will print:
// Successfully Update Table with value: 100
// Successfully Update Table with value: 50
```