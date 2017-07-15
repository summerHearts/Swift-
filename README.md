```
protocol Drivable {
    func selfCheck() -> Bool
    func startEngine()
    func shiftUp()
    func go()
}


class Roadster: Drivable {
    func selfCheck() -> Bool {
        return true
    }
    func startEngine() {}
    func shiftUp() {}
    func go() {}
}

class SUV: Drivable {
    func selfCheck() -> Bool {
        return true
    }
    func startEngine() {}
    func shiftUp() {}
    func go() {}
}

//MARK: - 结构体中的泛型
struct Point <T> {
    var x : T
    var y : T
}

protocol selecteItemProtocol {
  associatedtype  T
    func run() -> T
    func eat() -> T

}
//MARK: - 泛型与Protocol
class Person: selecteItemProtocol {
    func run() -> Person {
        return self
    }
    func eat() -> Person {
        return self
    }
}

class Dog: selecteItemProtocol {
    func run() -> Dog {
        return self
    }
    func eat() -> Dog {
        return self
    }
}



class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        var a = 10
        var b = 20
        
        exchangeNum(num1: &a, num2: &b)
        
        let point1 = Point(x: 1 , y: 2)
        print("\(point1.x), \(point1.y)")
        
        
        let stack = Stack<Int>()
        stack.push(2)
        stack.push(3)
        print(stack.pop()!)
        print(stack.pop()!)
        
        let person = Person()
        person.eat().run().eat().eat().run()
        
        test(a: person)
        
        //MARK: - 和面向对象中的运行时多态不同，泛型编程中调用方法的选择是在编译期完成的。编译器会根据参数的类型在正确的类中选择要调用的方法。这种行为，叫做编译期多态。
        

        drive(Roadster())
        drive(SUV())
        

    }

    //MARK: - 泛型，不特指某一种类型
    func exchangeNum<T>(num1 : inout T,num2 : inout T)  {
        let temp = num1
        num1 = num2
        num2 = temp
        print("\(num1), \(num2)")
    }
    
    
    func test<T>(a: T) where T: Person {
        
    }
    
    
    //但在泛型编程的世界里，故事就不同了。我们把之前的drive方法先改成这样：
    //通过泛型方式约定的接口，叫做implicit interface。 drive接受一个T类型的参数，只要这个类型支持了“自检”、“启动引擎”、“升挡”以及“前进”这4个操作，就可以把车开走。当然，和C++中的泛型编程不同，在语法上，Swift要求我们明确把刚才这个要求表达出来，而不能仅仅通过drive的实现隐式表达这个要求。而这，就是protocol的作用：
    func drive<T: Drivable>(_ car: T) {
        if !car.selfCheck() {
            car.startEngine()
            car.shiftUp()
            car.go()
        }
    }
}

class Stack <T>{
    var array: [T] = []
    
    func pop() -> T? {
        return array.removeLast()
    }
    
    func push(_ value : T) {
        array.append(value)
    }
}
```
