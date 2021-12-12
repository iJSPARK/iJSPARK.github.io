---
title : "Rock, Scissors, Paper Game"
categories : 
    - iOS
tag :
    - [Swift, Develop in Swift, Intro to App Development with Swift, Apple Eudcation]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
use_math: true
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

## Rock, Scissors, Paper Game
It is my first very simple project in iOS. The game is play with robot. This is in the education book 'Devlop in Swift' provided by Apple. However, there has no answer.

<img width="300" alt="Screen Shot 2021-12-12 at 4 35 59 PM" src="https://user-images.githubusercontent.com/92430498/145704355-be4d1e93-59ca-4942-867d-219cfefcb4d3.png">
<img width="300" alt="Screen Shot 2021-12-12 at 3 57 04 PM" src="https://user-images.githubusercontent.com/92430498/145703472-6ca23d62-9856-4e91-bbae-bba3db89d14a.png">
<img width="300" alt="Screen Shot 2021-12-12 at 4 35 59 PM" src="https://user-images.githubusercontent.com/92430498/145704355-be4d1e93-59ca-4942-867d-219cfefcb4d3.png">
<img width="300" alt="Screen Shot 2021-12-12 at 3 57 24 PM" src="https://user-images.githubusercontent.com/92430498/145703477-63f209ad-a2f9-448e-84b7-7fc2cca05171.png">

After writing the code, I try to study it by tearing the main source file one by one and situation in which the error occurred. The main source file is `Sign.swift`, `GameState.swift`, `ViewController` with `Main.storyboard`.  

<img width="271" alt="Screen Shot 2021-12-12 at 3 09 22 PM" src="https://user-images.githubusercontent.com/92430498/145702529-e9efa920-3c40-422e-8149-c31c6ffe041e.png">

---

**`Sign.swift`**

Enumeration in Swift much more powerful than other language. C enumerations assign related names to a set of integer values. However, In Swift, If a value (known as a raw value) is provided for each enumeration case, the value can be a string, a character, or a value of any integer or floating-point type.
Also, It has capabilities that see Properties, Methods, Initialization, Extensions, and Protocols. So in Swift, we have to implement program using Enumeration well. 
If use properties or method in `enum`, can safely drive values.

```swift
import Foundation
import GameplayKit // This framework contains an API to generate random numbers

let randomChoice = GKRandomDistribution(lowestValue: 0, highestValue: 2) // Generate instance

// Sign
enum Sign {
    case rock, scissors, paper
    
    // Return emoji
    var emoji:String {
        switch self {
        case .rock:
            return "✊"
        case .scissors:
            return "✌️"
        case .paper:
            return "✋"
        }
    }
    
    // Return matched result(game state)
    func match(_ opponent: Sign) -> GameState {
        switch self {
        case .rock:
            switch opponent {
            case .rock:
                return .draw
            case .scissors:
                return .win
            case .paper:
                return .lose
            }
        case .scissors:
            switch opponent {
            case .rock:
                return .lose
            case .scissors:
                return .draw
            case .paper:
                return .win
            }
        case .paper:
            switch opponent {
            case .rock:
                return .win
            case .scissors:
                return .lose
            case .paper:
                return .draw
            }
        }
    }
}

// Return robot random sign
func randomSign() -> Sign {
    let sign = randomChoice.nextInt() // random number 
    if sign == 0 {
        return .rock
    } else if sign == 1 {
        return .scissors
    } else {
        return .paper
    }
}
```

`nextInt()` is return value that integer in the range specified by the distribution’s `lowestValue` and `highestValue` properties. 

---

**`GameState.swift`**

```swift
import Foundation

// Game State
enum GameState {
    case start, win, lose, draw
    
    // Print matched result
    var description: String {
        switch self {
        case .start:
            return "Rock, Scissors, Paper!"
        case .win:
            return "You won!"
        case .lose:
            return "You lose!"
        case .draw:
            return "Draw!"
        }
    }
}
```

---

**`Main.storyboard`**

<img width="300" alt="Screen Shot 2021-12-12 at 5 20 08 PM" src="https://user-images.githubusercontent.com/92430498/145705472-ddfa34bd-a033-4262-b526-71838f867208.png">

---

**`ViewController`(Assistant)**

```swift
import UIKit

class ViewController: UIViewController {
    
    // Label
    @IBOutlet weak var robotSignLabel: UILabel!
    @IBOutlet weak var gameStateLabel: UILabel!
    
    // Button
    @IBOutlet weak var playAgainButton: UIButton!
    @IBOutlet weak var rockSignButton: UIButton!
    @IBOutlet weak var scissorsSignButton: UIButton!
    @IBOutlet weak var paperSignButton: UIButton!
    
    // Call afrer view on memory and just before appear view
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        view.backgroundColor = UIColor.gray // view
        robotSignLabel.text = "🤖" // back to the robot emoji
        
        // back to the scissors button after game play
        let text = NSAttributedString(string: "✌️", attributes: [.font: UIFont.systemFont(ofSize: 70)])
        
        scissorsSignButton.setAttributedTitle(text, for: .normal)
        gameStateLabel.text = GameState.start.description // Game State Lable
        rockSignButton.isHidden = false // Enable
        scissorsSignButton.isEnabled = true
        paperSignButton.isHidden = false // Enable
        playAgainButton.isHidden = true // Disable
    }
    
    // Action after button tap
    @IBAction func rockButtonTapped(_ sender: Any) {
        play(Sign.rock)
    }
    
    @IBAction func scissorsButtonTapped(_ sender: Any) {
        play(Sign.scissors)
    }
    
    @IBAction func paperButtonTapped(_ sender: Any) {
        play(Sign.paper)
    }
    
    @IBAction func palyAgainButtonTapped(_ sender: Any) {
        viewDidLoad() // reset
    }
    
    // Game play
    func play(_ userSign: Sign) {
     
        view.backgroundColor = UIColor(red: 0.65, green: 0.9, blue: 0.6, alpha: 0.95)
        
        let robotSign = randomSign() // Random robot Sign
        
        gameStateLabel.text = userSign.match(robotSign).description // Upload Game State Label
        robotSignLabel.text = robotSign.emoji // Upload robot sign
     
        let text = NSAttributedString(string: userSign.emoji, attributes: [.font: UIFont.systemFont(ofSize: 70)])
        
        scissorsSignButton.setAttributedTitle(text, for: .normal)
        rockSignButton.isHidden = true // Disable
        scissorsSignButton.isEnabled = false // Disable
        paperSignButton.isHidden = true // Disable
        playAgainButton.isHidden = false // Enable
    }
   
}
```

At first I got it wrong here. After game is over, sign which user select should appear at the position of the scissors. But emoji size is not 70.
Because scissorsSginButoon is not Label. `titleLabel` isn't correspond so emoji's font back to standard form. I need to know achitecture of Swift.

```swift
scissorsSignButton.setTitle(userSign.emoji, for: .normal)
scissorsSignButton.titleLabel?.font = UIFont.systemFont(ofSize: 70)
```

---

1. NSobject 
```swift
let text = NSAttributedString(string: userSign.emoji, attributes: [.font: UIFont.systemFont(ofSize: 70)])
        
scissorsSignButton.setAttributedTitle(text, for: .normal)
```

