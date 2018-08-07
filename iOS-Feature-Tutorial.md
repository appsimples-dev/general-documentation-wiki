## Developing a new feature
This tutorial will help you through the development of a new feature of an iOS project here at Outsmart. I'll propose some good practices and give tips on how to use our available tools. I'll try to explain some architecture decisions we made, and I'm open for suggestions and questions about it. There is a Base Project with some examples that will be referred during this tutorial. If you want to follow them, you can clone the base project from our remote repository into your local machine.

_tip: If you need help setting up a iOS development environment please check this tutorial on our wiki_
## User Stories
The most common case here at Outsmart is that during a Sprint Planning, your team will assign the responsability of developing a User Story in an ongoing project. In that case, the project will be already under development (maybe even in production) and at least some basic structure will be already available on the main branch. 
As an example, I'll guide you on how a simple User Profile could be created.

We use trunk based development, so to start your new feature, pull or clone the project, and then open a development branch from master. You should name your branch something like feature/userProfile, according to the user Story you are developing. For more information on git and the project structure please search our wiki and look at the readme at the base project.

If this is the first time running this project, you will need to run the "pod install" command on the terminal. Wait for the dependencies to download and open the .workspace file. Start coding!

## ViewController
It is not always the case, but usually a new feature requires a new Screen. On iOS that means that we should create a new ViewController. Create a new folder named "UserProfile" on the "Controllers" folder, we will put most of our files there. We have a base class for VCs, so create a new ViewController file, name it UserProfileViewController. Xcode will ask if you want to create a .xib file, confirm that you want that.

[[images/ios-tutorial/test2.gif]]

Now we have two files. The .swift file will have our screen logic, while the .xib file will have the interface layout. First, lets make some changes to the .xib file.

## Basic Auto-Layout

Here at Outsmart we use Xib for interfaces. We don't use Storyboards and we don't write all interfaces in code. We use autolayout to make our views responsive. Click on the .xib file to open the interface builder.
First add a UIImageView into our new screen. Use the Interface Builder and drag a UIImageView into the ViewController. Click on the add contrains button and add constrains to the top spacing, left spacing, height and width. 

[[images/ios-tutorial/test3.gif]]

_tip: The basic constraint structure is to have 4 constraints (2 vertical, 2 horizontal) define the view size and position on screen, no more, no less. Use this rule to avoid having constraint errors. _

_advanced tip: That is not enough to make automatic dimension views such as UITableViewCell and UIScrollView._

Next add a UILabel to the screen, a little to the right of the ImageView. Click on the add contrains button and add constrains to the top spacing, left spacing, right spacing and height. Add another UILabel to the right of the ImageView, and below the first UILabel. Add constrains to the top spacing, left spacing, right spacing and height.

[[images/ios-tutorial/test4.gif]]

Your Interface Builder should look similar to this:

[[images/ios-tutorial/img1.png]]

You can click on any of the views, change the constraints and see how the screen rearenge in real time. To change the constraints, select the view in the interface builder, click on the ruler tab, scroll down and change the constatnt value of any constraint.

[[images/ios-tutorial/test5.gif]]

_tip: It's very common to have a screen with a long/infinite list of components, such as a user list or a feed of news. In that case we use the power of the UITableView. Since our views are very simple, we didn't need to use a TableView, but if you want to learn more about it please search our wiki or ask your colleagues._

_tip2: This is a very naive  implementation of a interface using the interface builder. We prefer to use our own custom components, themes and more complex architectures. Since this is an introductory tutorial, the focus was on an overview of the implementation flow, so we did not focus on those complex interface subjects. On a real project you will probably use a different strategy to handle the user interface, ask your collegues before advancing._

## Variables, Outlets and Simple View Configuration

Now we need to link the screen that was just created with our .swift class, so that the visual components can interact with code. To do so is very simple in Xcode.

First we create three variables in our UserProfileViewController class, on for each view on our screen. Write the following code on the .swift file.

```swift
class UserProfileViewController: UIViewController {
    
    @IBOutlet weak var profileImage: UIImageView?
    @IBOutlet weak var fullNameLabel: UILabel?
    @IBOutlet weak var hometownLabel: UILabel?
    ...
```

A little explanation on what we just did.

* "@IBOutlet": this is  a special property that allow us to interact with those variables on the Interface Builder.
* weak: this is a memory management control. Since our views were created using the interface builder, they will be added as a subview of our screen main view (a strong property of the viewcontroller class). This creates a strong link between the view hierarchy, so a weak link is enough. It's not important to understand this right now, but if you want to ask on the iOS chapter about memory management tips.
* var: this is a simple variable.
* userProfileImage: this is the name of the variable
* :UIImageView?: this is the type of the variable. The "?" at the end indicates that this is an optional variable, so it's value may be nil. Please use optional types for the interface builder variables instead of implicitly unwrapped optionals.

Now we need to link between the code and the interface builder. Open the interface builder, click on the UserProfileViewController and then on the connections inspector tabs. There you will see our variables, just drag from the small circle to each view. You can follow those steps on the gif below.

[[images/ios-tutorial/test6.gif]]

Since our iboutlets are now linked, we can go back to our .swift file and add some code. We will change the text and font color of our labels, and use the alamofire image pod to load an image from an url on our ImageView. Write the code below on the viewDidLoad method.

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    fullNameLabel?.text = "John Doe"
    hometownLabel?.text = "New York"
    if let url = URL(string: "https://picsum.photos/200?image=5") {
        profileImage?.af_setImage(withURL: url)
    }
}
```

_tip: We can also change the properties of our view on the interface builder. This creates an ambiguity between configurations in code and configurations in the interface builder. To resolve this, always config your view's properties on code. Using our custom components and themes make this easier._

## Coordinator and Navigation

To make our view controller accessible in the app flow, we need to create an instance of it and present it using either the push method (in case of a uinavigationcontroller navigation) or the present (in case of a modal navigation).

We have a special class called coordinator to manage such navigations. The reason we use it is to decouple the source view from the view we wish to navigate. Also, the coordintor class helps organizing and finding  all possible navigation flows.

_ps: Our implementation of the coordinator class should not be confused with the coordinator pattern that has gained some popularity in the iOS community. For our projects the coordinator pattern proved too complex and time consuming; the rewards were not big enough to justify the effort. We may reconsider using the coordinator patter in the future._

First, lets create a custom initiation function on our view controller. This is a good practice, it makes it clear to anyone using this class what are the required parameters to start. It also makes the coordintor class more clean, and our code more easy to refactor if the requirements change. Create a new function on the UserProfileViewController class like this.

```swift
var userId: String?
    
class func newUserProfile(userId: String) -> UserProfileViewController {
    let instance = UserProfileViewController(nibName: "UserProfileViewController", bundle: nil)
    instance.userId = userId
    return instance
}
```

Please take note that we created a required parameter on our function, and stored it on a variable. This will be a reference to the user that our profile screen is presenting. Since this is a required parameter, if everyone uses that function when navigating to this screen will have to specify a userId, and we defend ourself from having a profile screen without a user reference.

Now open the AppCoordinator.swift file and add a startUserProfile function. This is will be our navigation function, anyone that wants to navigate to the UserProfile screen should call this function on the AppCoordinator class.

```swift
class func goToUserProfile (userId: String, parent: Any) {
    let vc = UserProfileViewController.newUserProfile(userId: userId)
    AppCoordinator.startViewController(newVC: vc, parent: parent)
}
```

First we create a new instance of our view controller, then we use the start function to include the new instance on the navigation hierarchy. This is a comodity function that we created, simplifying the process of navigating to a new vc. The parent param will normally be a UINavigationController or a UIViewController.

Now you can choose any view controller in our app and use the coordinator to navigate to our profile screen. As an example open the StandardViewController.swift file, and modify the flash function as follows.

_tip: Use cmd + shift + F to open the project search tab and find any file you want to edit._

```swift
func flash() {
    AppCoordinator.goToUserProfile(userId: "0", parent: navigationController)
}
```

## Run and Debug

Now that our new screen was added to the app navigation we can run the project on the simulator and test our code. Choose any simulator and click on the run button to start our app. Click on the thunder icon to open our screen and see if its working as intended.

[[images/ios-tutorial/test7.gif]]

Now is a good time to learn about debuging. Debuging is a very important skill for any software engineer, so it's better if we start learning about it as soon as possible. Xcode make it easy to make some simple debuging. Select the UserProfileViewController.swift file and click on the line number to create a new breakpoint.

[[images/ios-tutorial/test8.gif]]

Now if you run the project again, the code will stop right before our screen shows on the app. Then you can check the view controller instance variables and see that the userId that we sent before is set with the correct value. To resume the simulator, click on the resume (play)  button.

[[images/ios-tutorial/test9.gif]]

## View Model

One of the most important aspects of a good codebase, is the separation of responsability between layers of the application. Here at Outsmart we use the MVVM architecture to separate view logic from business logic on iOS apps. Its important to follow this pattern to make our app cleaner, more testable and maintanable.

We will start by making a simple view model to store the user data. This is a very important step, and somewhat confusing for begginers. If you don't understant everything on your first run its ok! Your collegues can help you understand it, and there is a lot of material online to study.

First lets create a new swift file, name it UserProfileViewModel.swift. We will start by creating a UserProfile datasource and delegate protocols. Write this code on the new file.

```swift
protocol UserProfileDatasource: class {
    func userFullname() -> String
    func userHometown() -> String
    func userProfilePicURL() -> URL?
}

protocol UserProfileDelegate: class {
    func fetchUserProfileData()
}
```

Here is the definition of swift protocols according to apple documentation.
> A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.

The reason we use protocols is to decouple the view model from the view controller. By using protocols we can change either the view controller or the view model implementations more easily. Lets create another protocol, the UserProfileFeedback. That protocol will be implemented by the view controller to respond to the viewmodel's events.

```swift
protocol UserProfileFeedback: class {
    func didLoadUserProfileData(error: Error?)
}
```

Now that we have our protocols in place we can start coding our view model. For now we will create placeholders implementations and check if all is working. Write the following code on the UserProfileViewModel.swift file below the protocols.

```swift
class UserProfileViewModel {
    var userId: String?
    weak var feedback: UserProfileFeedback?
    
    class func initWith(userId: String, feedback: UserProfileFeedback) -> (UserProfileDelegate, UserProfileDatasource) {
        let instance = UserProfileViewModel.init()
        instance.userId = userId
        return (instance, instance)
    }
}

extension UserProfileViewModel: UserProfileDatasource, UserProfileDelegate {
    func userFullname() -> String {
        return "Maria Santana"
    }
    
    func userHometown() -> String {
        return "Appleville"
    }
    
    func userProfilePicURL() -> URL? {
        return URL(string: "https://picsum.photos/200?image=6")
    }
    
    func fetchUserProfileData() {
        //Later we will call API here
        feedback?.didLoadUserProfileData(error: nil)
    }
}
```

Here, is important to note that the feedback variable was declared as weak and of the UserProfileFeedback? type. Also, we are using a protocol type instead of the ViewController type to decouple our layers. The variable is weak to avoid a reference cycle that could cause a memory leak. Since the variable is weak, we need to make it an optinal type. Please follow this pattern when creating feedback variables on all view models.

_tip: To understand more about memory management, reference counting, strong reference cycles and memory leaks, please see our wiki!_

## Refactoring

Now that we created our view model we will use it to store and provide information about the user. To do so we will make our first Refactor! Refactoring is a good coding practice. To understand more about it see this link https://en.wikipedia.org/wiki/Code_refactoring.

We will rewrite our view controller. Replace all the code on the UserProfileViewController.swift file by the following.

```swift
import UIKit

class UserProfileViewController: UIViewController {
    
    @IBOutlet weak var profileImage: UIImageView?
    @IBOutlet weak var fullNameLabel: UILabel?
    @IBOutlet weak var hometownLabel: UILabel?
    
    var userId: String?
    var datasource: UserProfileDatasource?
    var delegate: UserProfileDelegate?
    
    class func newUserProfile(userId: String) -> UserProfileViewController {
        let instance = UserProfileViewController(nibName: "UserProfileViewController", bundle: nil)
        instance.userId = userId
        return instance
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        (delegate, datasource) = UserProfileViewModel.initWith(userId: "0", feedback: self)
        delegate?.fetchUserProfileData()
    }
}

extension UserProfileViewController: UserProfileFeedback {
    func didLoadUserProfileData(error: Error?) {
        if let error = error {
            alert(title: "Error!", message: error.localizedDescription, actions: nil)
        }
        fullNameLabel?.text = datasource?.userFullname()
        hometownLabel?.text = datasource?.userHometown()
        
        if let url = datasource?.userProfilePicURL() {
            profileImage?.af_setImage(withURL: url)
        }
    }
}
```

Now our view controller and view model are only interacting via protocol methods. Congrats! Your code is more decoupled now!

Test again by running the code on the simulator and check if everything is working as intended.

[[images/ios-tutorial/img2]]

## End of Part 1
Right now we have a working User Profile implementation, with a clean decoupling of the ViewModel and ViewController layers. Congrats! If you followed everything up to here you lerned a lot of what is needed to be a iOS developer.

On the next part of the tutorial we will learn how to integrate our simple Profile Screen with an external API. Click here to take the next app into iOS mastery!