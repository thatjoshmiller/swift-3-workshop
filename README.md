<<<<<<< HEAD
##Lab-3-prework  
####1. Ensure in Xcode that you are on the **lab-3** branch.  

##Lab-3  
> In this lab we will start to build out a ToDo list Application. This lab will help to teach the most-commonly used UIKit elements.  

> Before we get started I would recommend if you are not familiar with the concept of MVC, to read this article: [MVC Design Pattern](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html)  

> From this point forward, I may refer to specific types as as either Model, View or Controller objects.

> I would also recommend setting up your file structure on the left to follow this convention and to keep consistent with the rest of this tutorial, but it is not a requirement.  

Following the MVC design pattern, you're file structure should appear similar to this:  
![Imgur](http://i.imgur.com/6UMSbGF.png)  


##Building Our Model  
First we need to create out Todo class.  

In Xcode, we will create a new separate swift file for this class and we will add it to our **Model** group.  
To do this, we can use a shortcut for creating new files, `cmd+N`, or we can use the menu bar and navigate to *File > New > File...*:  
![Imgur](http://i.imgur.com/XMsTzWQ.png)  

Now we can select *Swift File* from the menu:  
![Imgur](http://i.imgur.com/wtXbLqu.png)  

Create your new file and name it `Todo`.  
We should now have an empty `.swift` file to work with.  
![Imgur](http://i.imgur.com/lTFi3OR.png)  

We will define the class underneath the `import Foundation` line by typing the following:
```swift
class Todo{
    
    var text: String
    
    init(text: String) {
        self.text = text
    }
    
}
```  

We will need a way to identify each Todo item later, so lets create another new **Swift File** called `Identity`. 

`Identity` will be a simple protocol that will require a single variable called `identifier`.  

In **Identity.swift**, add the following code below your imports:  
```swift
protocol Identity
{
    var identifier: String { set get }
}
```  

> The above is saying in order to conform to the `Identity` protocol, we must implement a variable called `identifier` and are responsible for its setter and getters. For more on `set` and `get`, read more into protocols here: [Apple documentation](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html) - Scroll down to **Property Requirements**.  

Now, back in out `Todo.swift` file, inside out `Todo` class, lets add the required variable for the `Identity` protocol.  
```swift
var identifier: String
```  

We can also add the following line inside our `init`:
```swift
self.identifier = UUID().uuidString
```  

> The above line uses the UUID Struct to get a unique ID string for our `identifier` variable.  

At this point, your `Todo` class should resemble the following:  
![Imgur](http://i.imgur.com/6TSx0GR.png)  

Now we need to create a `TodoList`.  
This class will manage adding, removing, and storing our `Todo` items.  

Create a new swift file called `TodoList`.  
Click on the `TodoList.swift` file and under the `import Foundation` add:  
```swift
class TodoList{
    
   var allTodos = [Todo]()
    
}
```  

> This will create a new class type called `TodoList`. We also add an array of `Todo` objects. This will be all of our `Todo`'s the user adds.

Next, we will make this class a singleton.  

> Singletons are a design pattern that is common in most languages. This means that there will be only 1 single instance of this class called `shared` that we can access anywhere in our application. This instance is created the first time the `shared` instance is accessed. Singletons should be used sparingly and are just one way of passing data around in your applications.  

> For more information on singleton pattern in iOS, check out this article. Keep in mind the article was written before Swift 3 but is still relevant: [Swift Singletons](http://krakendev.io/blog/the-right-way-to-write-a-singleton)  

Add the following line inside your class declaration, above the `allTodos` array:  
```swift
static let shared = TodoList()
```  

> The static keyword means that this applies to the type itself, not any particular instance of the class. This will make more sense when we use `shared` later in this tutorial.  

We also want to make sure that only **1** instance of this class gets created so we will add a private initializer.  

```swift
private init(){}
```  

At this point your `TodoList` class should look like this:  
![Imgur](http://i.imgur.com/bXSUJzr.png)  

Now, we have 5 Methods we need to add to this class.  
1. `add(todo:)`  
2. `remove(todo:)`  
3. `removeAll`  
4. `getTodoAt(index:)`  
5. `count()`  

First, lets add our `add(todo:)` function:  
```swift
func add(todo: Todo){
    self.allTodos.append(todo)
}
```  

> As we saw earlier, this will allow us to add items to our `allTodos` array.  

Next, we will add our `remove(todo:)` function right below the above `add` function.  
```swift
func remove(todo: Todo){
        self.allTodos = self.allTodos.filter{ (item) -> Bool in
            return item.identifier != todo.identifier
        }
    }
```  
> The `filter` function provides a closure for us to do some processing of the objects in the array. This is an example of what is called a higher order function. This is something we go into detail about in the iOS 401. But, for more information on Swift higher order functions, check out this article:[Higher Order Functions](https://www.weheartswift.com/higher-order-functions-map-filter-reduce-and-more/)  

Now, let's add the `removeAll`, `getTodoAt`, and `count` functions below the `remove` function:  
```swift
func removeAll(){
    self.allTodos.removeAll()
}

func getTodoAt(index: Int) -> Todo{
    return self.allTodos[index]
}

func count() -> Int{
    return self.allTodos.count
}
```  

> All of these functions should be fairly straight-forward. They access and manipulate the data for us on the `allTodos` array.

Your `TodoList` class should now look like this:  
![Imgur](http://i.imgur.com/TJvuCpb.png)  

This completes the building of our model.  
We will continue adding some additional functionality in the next lab.  

##UIView  
> The UIView class defines a rectangular area on the screen and the interfaces for managing the content in that area.  

> UIViews are how we typically display content to the user.  

> Every UIView can be a subview of another UIView, or have other UIView’s as children. You can think of the view hierarchy similar to a family tree.  

For more on UIView's read: [UIView Documentation](https://developer.apple.com/reference/uikit/uiview)  

> We will interact with these views, both directly and indirectly, as we build our application.  

In our project, click on the ViewController.swift file on the left here:  
![Imgur](http://i.imgur.com/n4UGja0.png)  

##UIViewController  
> The UIViewController class provides the infrastructure for managing the views of your iOS apps. A view controller manages a set of views that make up a portion of your app’s user interface. It is responsible for loading and disposing of those views, for managing interactions with those views, and for coordinating responses with any appropriate data objects. View controllers also coordinate their efforts with other controller objects—including other view controllers—and help manage your app’s overall interface.

> Every UIViewController has a **Root View**. This is the primary view that this controller is a wrapper around.

For more on View Controllers, read Apple's [View Controller Programming Guide for iOS
](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457).  

We can explore our View Controller's root UIView.  
To do this, give yourself some space within the scope of the `viewDidLoad` function.  
Add the following line of code:  
```swift
self.view
```  
At this point, you should see something similar to this:  
![Imgur](http://i.imgur.com/10jVXsk.png)  

Don't mind the error, and we can not `Option+click` on `view` to get a brief description of `self.view`.  

It should appear like this:  
![Imgur](http://i.imgur.com/BUHrFxg.png)  

If you go to the bottom of this window, you can click on *Property Reference* at the bottom to jump into the documentation.  

When you are done researching, we can remove the `self.view` code from before.  

> There are 2 primary types of view controllers. There are **content** view controllers that present content to the user, and there are **container** view controllers that manage other view controllers and how they are displayed to the user.  

##UINavigationController  
> UINavigationController is a container view controller. It manages *child* view controllers. 

> The UINavigationController class implements a specialized view controller that manages the navigation of hierarchical content. This navigation interface makes it possible to present your data efficiently and makes it easier for the user to navigate that content. You generally use this class as-is but you may also subclass to customize the class behavior.  

> For more information on UINavigationController: [UINavigationController Documentation](https://developer.apple.com/reference/uikit/uinavigationcontroller)  

There are 2 ways to incorporate a UINavigationController into your application. We can do it using Storyboards or build it programmatically. For today, we will use Storyboards.  

> In the iOS 401, we spend an entire week building out an application programmatically, including the user interface.  

On the left, click on **Main.storyboard**. You should now see something similar to this:  
![Imgur](http://i.imgur.com/sBaYTy8.png)  

Now, click on the top bar of the UIViewController:  
![Imgur](http://i.imgur.com/ShhBqIr.png)  

Then up in the menu bar, navigate to *Editor > Embed In > Navigation Controller*.  
![Imgur](http://i.imgur.com/ekh8tCt.png)  

Your storyboard should now look like this:  
![Imgur](http://i.imgur.com/7l6RmXZ.png)  

##UITableView  

> A table view displays a list of items in a single column. UITableView is a subclass of UIScrollView, which allows users to scroll through the table, although UITableView allows vertical scrolling only. A table view is made up of zero or more sections, each with its own rows. Sections are identified by their index number within the table view, and rows are identified by their index number within a section. Any section can optionally be preceded by a section header, and optionally be followed by a section footer.

> For today's workshop we are going to build a dynamic tableview that populates a given number of cells depending on the number of todo's the user creates.

In `Main.storyboard`, zoom in a bit to your ViewController, **NOT** the Navigation Controller.  
You should see something similar to this:  
![Imgur](http://i.imgur.com/YdsREo9.png)  

Drag out a tableView from the bottom right list of UIElements in Xcode:  
![Imgur](http://i.imgur.com/tfGX2sf.png)  

Place the tableView on top of the ViewController like this:
![Imgur](http://i.imgur.com/7KD3Zd1.png)  

Drag the corner edges of the tableview to fill the ViewController from below the navigation bar to the bottom like this:  
![Imgur](http://i.imgur.com/xbGB1R7.png)  

if you see a gap at the top of your tableview, click on the top bar of your ViewController to highlight it.  
Go to the attributes inspector on the right, and make sure that *Adjust Scroll View Insets* is unchecked.  
![Imgur](http://i.imgur.com/OTZLRAm.png)  

##UITableViewCell  
> This class defines the attributes and behavior of the cells that appear in a UITableView. This class includes properties and methods for setting and managing cell content and background (including text, images, and custom views), managing the cell selection and highlight state, managing accessory views, and initiating the editing of the cell contents.  

> When creating cells, you can customize them yourself or use one of several predefined styles. The predefined cell styles are the simplest option. With the predefined styles, the cell provides label and image subviews whose positions and styling are fixed.  

In storyboard, let's get a Table View Cell from the UIElements in the bottom right corner of Xcode.  
![Imgur](http://i.imgur.com/wstkHnG.png)  

Now drag out the cell and drop it on top of the TableView on our ViewController.  
![Imgur](http://i.imgur.com/CbJSOKn.png)  

In the *Attributes Inspector* on the right, change the style of the cell from *Custom* to *Basic* and set "cell" as the Identifier.  

So, your attributes should look like this:  
![Imgur](http://i.imgur.com/MtXCyHk.png)  

Now, we can open what is called the *Assistant Editor*.  
To do this, `option+click` on the `ViewController.swift` file in our project on the left.  

> The Assistant Editor allows us to do a side-by-side view of 2 different files. In our case, the `ViewController.swift` file and our `Main.storyboard` file.  

Xcode should now look similar to this:  
![Imgur](http://i.imgur.com/UaXvw0I.png)  

In the View Heirarcy on the left, next to our files, click on the Table View.  

Now, `ctrl+drag` from the Table View to your code, right above the `viewDidLoad` method.  

When doing this, you should see a blue connection from the storyboard to your code.  

After you let go to make the connection, you should see a box appear.  
![Imgur](http://i.imgur.com/OFQS48y.png)  

This will allow you to enter a name for this UITableView. For today, we will call this `tableView`.  

Type in `tableView` and click `connect`.  

You should now see an outlet to your `tableView` in the code on the right.  

> This is how we can connect many objects from storyboards to code. This allows us to build complex and dynamic UI.  

![Imgur](http://i.imgur.com/Av9RTEG.png)  

Now, in the top right of Xcode, click on the button that looks like multiple lines, next to the 2 blue rings.  

This will take us out of the assistant editor and will return us to a single page view.  

![Imgur](http://i.imgur.com/GwtcGpE.png)  

Click on the `ViewController.swift` file on the left to bring the code back up.  

Now, in order to provide, or be the "source" of our `tableView`'s data, we need to make our `ViewController` class conform to the `UITableViewDataSource` protocol.  

This should look similar to what we did earlier in the day with protocols.  

At the top of the file, under the `import UIKit` line, where the class is declared, Add: `UITableViewDataSource`  
![Imgur](http://i.imgur.com/ljpEzzr.png)  

> NOTE: The error we get should be because, as stated, we are not properly conforming to the `UITableViewDataSource` protocol. This protocol requires 2 methods to be implemented. These methods are used to provide the tableView with it's data.

Under the `didRecieveMemoryWarning` function, begin to type `tableView`, you should see the following list of methods appear:  
![Imgur](http://i.imgur.com/0Sav9FY.png)  

The 2 required methods we must implement to be our `tableView`'s dataSource are `tableView(cellForRowAt indexPath:)` and `tableView(numberOfRowsInSection:)`.  

Allowing autocomplete to help us out we should end up with the following:  
![Imgur](http://i.imgur.com/EodqlH3.png)  

First, we can add 1 line of code to `numberOfRowsInSection` to complete the method.  

Replace the `code` keyword with the following line:  
```swift
return TodoList.shared.count()
```  

Then, inside of the `cellForRowAt` method, add the following line:  
```swift
let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
```  

> UITableViewCell's are reused by UITableView's as needed. This is very powerful because it helps to keep our memory footprint minimal while still being very fast.  

> The "cell" identifier we provided **MUST** match the identifier you assigned in the attributes inspector in `Main.Storyboard`.  

> Also note that `cellForRowAt` gets called for **Every** cell as its about to be displayed on screen.  

After the above line of code, add the following:  
```swift
let todo = TodoList.shared.getTodoAt(index: indexPath.row)
cell.textLabel?.text = todo.text
return cell
```  

> The above code gets a reference to the todo item for this specific cell and assigns it's `.text` String to be the cell's `textLabel` text. Finally, this method needs to return a `UITableViewCell`, so we return the newly created and configured cell.  

One other very important thing we need to do to get this to work, is assign the `dataSource` of the `tableView` to be this `ViewController` class.  

We can do this by adding the following line inside `viewDidLoad`, below `super.viewDidLoad(animated)`:  
```swift
self.tableView.dataSource = self
```  

Your `ViewController.swift` class should now look similar to this:  
![Imgur](http://i.imgur.com/uEASOhM.png)    

Now we are almost done. We just need to give ourselves some starting `Todo` items so we can see them displayed in the `tableView`.  

To do this, add the following code beneath the above line inside `viewDidLoad`:  
```swift
for number in 1...5{
    let todo = Todo(text: "Todo Number \(number)")
    TodoList.shared.add(todo: todo)
}
```  

> This will give us 5 default "Todo" objects to populate our table with.  

Build and run to see the result.  

####Coding Challenges  
> If you have extra time, here are some good challenges to attempt on your own.  

1. Look into subclassing `UITableViewCell` and create your own custom subclass.  

2. Research `UITablViewHeaderView` and `UIImageView` and give your Todo list a custom `HeaderView`.  
=======
#Swift 3 Workshop  
> This workshop is intended for individuals who want a ground-up overview of Swift 3. This workshop will primarily benefit people with some existing knowledge of Swift or some other programming language.  This workshop is not meant for people with no software development experience.  

##Prework  
1. Please make sure to have Xcode 8 downloaded and installed on your machine.  
2. Make sure you have a [GitHub](https://github.com) account.  
3. Fork this repo.  
4. Make sure to have [Homebrew](http://brew.sh/) and **git** installed on your machine by running the following from the command line:  
`brew install git`
5. Change from `master` to `lab-1` branch to get started.  

##Slidedeck
[Swift-3-workshop](https://www.icloud.com/keynote/0HfZtjZ-IAGhLX8mKhxh9kXKQ#Swift-3-workshop) 

>>>>>>> fb7a114f1db3c5e40ec5d68aa67d6b73a9fc0833
