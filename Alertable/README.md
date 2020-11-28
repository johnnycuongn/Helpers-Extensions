# Alertable

## Use

Make simple alert on View Controller simpler

## Code

```Swift
import UIKit

public protocol Alertable {}
public extension Alertable where Self: UIViewController {
    
    func showAlert(title: String = "", message: String, preferredStyle: UIAlertController.Style = .alert, completion: (() -> Void)? = nil) {
        let alert = UIAlertController(title: title, message: message, preferredStyle: .alert)

	// Add your own action
        alert.addAction(UIAlertAction(title: "OK", style: UIAlertAction.Style.default, handler: nil))

        self.present(alert, animated: true, completion: completion)
    }
}
```

## Implementation
```Swift
class ViewController: UIViewController, Alertable {
    
    func aFunction() {
        showAlert(message: "HELLO")
    }
    
}
```