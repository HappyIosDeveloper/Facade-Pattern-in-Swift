# Facade Pattern in Swift

## Simple example of Facade pattern written in Swift

## You can also download the demo playground project.

    import UIKit
    
    struct User: Equatable {
        var name: String
        var email: String
    }
    
    class UserManager {
    
        var users: [User]
    
        init(users: [User]) {
            self.users = users
        }
    
        func getAllUsers()-> [User] {
            return users
        }
    
        func add(user: User) {
            users.append(user)
        }
    
        func removeUser(user: User) {
            if let index = users.firstIndex(where: {$0 == user}) {
                users.remove(at: index)
            }
        }
    }
    
    class NetworkLayer {
    
        static let shared = NetworkLayer()
    
        func add(user: User) {
            // api call to add user
        }
    
        func removeUser(user: User) {
            // api call to remove user
        }
    }
    
    class FacadeUserManager {
    
        let manager: UserManager
    
        init(manager: UserManager) {
            self.manager = manager
        }
    
        func add(user: User) {
            manager.add(user: user)
            NetworkLayer.shared.add(user: user)
            // ... doing more complicated task
        }
    
        func remove(user: User) {
            manager.removeUser(user: user)
            NetworkLayer.shared.removeUser(user: user)
            // ... doing more complicated task
        }
    }
    
    // using the facade:
    let newUser = User(name: "Ahmadreza", email: "ahmadreza@gmail.com")
    let users = [
        User(name: "Jack", email: "jack@gmail.com"),
        User(name: "John", email: "john@gmail.com"),
        User(name: "Sarah", email: "sarah@gmail.com")
    ]
    let manager = UserManager(users: users)
    let facade = FacadeUserManager(manager: manager)
    
    // Here facade is doing the all complicated tasks
    facade.add(user: newUser)
    
