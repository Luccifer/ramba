![](https://img.shields.io/badge/swift-3.0.1-green.svg)
![](https://img.shields.io/badge/VIPER-generamba-orange.svg)


# HOWTO:


1) [Install Generamba](https://github.com/rambler-digital-solutions/Generamba)

2) [Initialize Generamba](https://github.com/rambler-digital-solutions/Generamba/wiki/Available-Commands#basic-generamba-configuration) in your project's folder and install templates from this repo

3) open terminal and execute this in the same project's folder: 
```bash
$ generamba gen Module swifty_viper
```

The new VIPER Module/Submodule(whatever) will be generated in your project's folder with structure:

<!-- AUTO-GENERATED-CONTENT:START (DIRTREE:dir=./) -->
```
Module
  ├── View
  │    └── ModuleViewController.swift
  ├── Presenter
  │    └── ModulePresenter.swift
  ├── Interactor
  │    └── ModuleInteractor.swift
  ├── Router
  │    └── ModuleRouter.swift
  ├── Configurator
  │    ├── ModuleConfigurator.swift 
  │    └── ModuleInitializer.swift
  └── Protocols
       └── ModuleProtocols.swift
```
<!-- AUTO-GENERATED-CONTENT:END -->

# Example Content:

```bash
generamba gen Test swifty_viper
``` 

###View -> TestViewController.swift:

```Swift
import UIKit

class TestViewController: UIViewController, TestViewInput {

    var output: TestViewOutput!

    // MARK: Life cycle
    override func viewDidLoad() {
        super.viewDidLoad()
        TestModuleConfigurator().configureModuleForViewInput(self)
        output.viewIsReady()
    }


    // MARK: TestViewInput
    func setupInitialState() {
    }
}
```

###Presenter -> TestPresenter.swift:

```Swift
class TestPresenter: TestModuleInput, TestViewOutput, TestInteractorOutput{

    weak var view: TestViewInput!
    var interactor: TestInteractorInput!
    var router: TestRouterInput!

    func viewIsReady() {

    }
}
```

###Interactor -> TestInteractor.swift:

```Swift
class TestInteractor: TestInteractorInput {

    weak var output: TestInteractorOutput!

}
```

###Router -> TestRouter.swift:

```Swift
import UIKit

class TestRouter: TestRouterInput {

	weak var view: UIViewController?
}
```

###Configurator -> TestConfigurator.swift:

```Swift
import UIKit

class TestModuleConfigurator {

    func configureModuleForViewInput<UIViewController>(_ viewInput: UIViewController) {

        if let viewController = viewInput as? TestViewController {
            configure(viewController)
        }
    }

    private func configure(_ viewController: TestViewController) {

        let router = TestRouter()
        router.view = viewController

        let presenter = TestPresenter()
        presenter.view = viewController
        presenter.router = router

        let interactor = TestInteractor()
        interactor.output = presenter

        presenter.interactor = interactor
        viewController.output = presenter
    }

}
```

###Configurator -> TestInitializer.swift:

```Swift
import UIKit

class TestModuleInitializer: NSObject {

    //Connect with object on storyboard
    @IBOutlet weak var viewController: TestViewController!

    override func awakeFromNib() {

        let configurator = TestModuleConfigurator()
        configurator.configureModuleForViewInput(viewController)
    }

}
```

###Protocols -> TestProtocols.swift:

```Swift
protocol TestViewInput: class {

    func setupInitialState()
}

protocol TestViewOutput {

    func viewIsReady()
}

protocol TestModuleInput: class {

}

protocol TestInteractorInput {

}

protocol TestInteractorOutput: class {

}

protocol TestRouterInput {

}
```

Russian Federation, Moscow, 2017
Karpushkin G.A
