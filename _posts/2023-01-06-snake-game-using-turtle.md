---
layout: post
title: Building Snake Game Using Python Turtle
subtitle: Each post also has a subtitle
categories: [coding, game]
tags: [coding, object-oriented programming, oop, game]
---

<p align="center">
 <img src="/assets/images/banners/snake_game/cover_game.jpg" width="600">
 Source: <a href="https://www.emulatorpc.com/snake-game//">emulatorpc.com</a>
</p>


### **Overview** 
Being non-developer, but always dealing with code here and there, I have been feeling, my coding skill is lacking. So, this week, I wanted to refresh my programming skill and revisit a programming concept I have learned in the past, namely OOP (Object-Oriented Programming). I did some quick research on the small project I could take as exercise and come across old game I used to play on my old 3350 Nokia. Yes, snake game. 

I use python as the main programming language and found that it comes with pre-installed library, turtle, thus it doesn’t need to be installed externally. Turtle is a pre-installed Python library that enables users to create pictures and shapes by providing them with a virtual canvas. As this game is very simple and we all might be familiar with the rules, I decided to re-create this game using python and turtle. 
Turtle documentation can be found [here](https://docs.python.org/3/library/turtle.html).

<p style="margin-bottom:-30px"></p>

### **Object-Oriented Programming**
As opposed to procedural, object-oriented programming uses concept of class and object. I always imagine that class is like building blocks when playing or building Lego. It is reusable piece of ‘tools’ that allows me to re-create an object without having to build everything from the scratch, which is why it is called ‘blueprint’. I really liked this concept when it was introduced to me at the first time because it requires less line of code to achieve the same goal. In addition, my code appears to be tidy and neat because OOP hides the ‘main machine’ on the back. From developer POV, OOP creates more maintainable code.

Class has attributes and methods. Attributes are everything that an object has, such as manufacturing year, color, shape, etc., while methods represent the abilities of an object; can it fly, talk or cook? It is built using method called constructor '__init__'. When this method is called, it allows the class to initialize the attributes of the class.

Class also allows us to be very flexible because it could inherit other's class attributes and methods or called parent class. For example, the scoreboard, food and speed classes as subclasses (scoreboard.py, food.py, speed.py) inherits all attributes and methods from Turtle class as their parent class. This means, we could use all functionalities from Turtle class without repeating code and custom it as we wish. In python, this concept is called 'inheritance'. What we need to do is only importing the class we want to inherit and calling the '__super()__' method. Please note that I only use single inheritance here.

<p style="margin-bottom:-30px"></p>

### **Building Snake Game**
For this game, I broke down the steps into smaller ones to identify what I should do first, second, third and so on. In addition, I divided the files into several files that it won’t clutter the main file (‘app.py’). At the end, I happened to have five separate python files.

<p align="center">
  <img src="/assets/images/banners/snake_game/python_files.png" width="200">
</p>

Beforehand, turtle library (also time) needs to be imported.

<p align="center">
  <img src="/assets/images/banners/snake_game/library.png" width="200">
</p>

<p style="margin-bottom:-30px"></p>

#### 1.	Setting up the screen

<p align="center">
  <img src="/assets/images/banners/snake_game/part1a.png" width="600">
</p>

To prevent the screen from flashing away immediately, ‘exitonclick’ needs to be added at the bottom of the app.py file.

<p align="center">
  <img src="/assets/images/banners/snake_game/part1b.png" width="600">
</p>

#### 2.	Creating snake body

I made the initial snake or seed of snake using only 2 blocks of square by initializing turtle object from turtle class. The default size of each block is 20 x 20. The starting position of initial snake is supposed to be at the center of the screen (default is (0,0)). Here tuple is used to define the starting point.

<p align="center">
  <img src="/assets/images/banners/snake_game/part2a.png" width="600">
</p>

Here I initialized the snake object from the snake class that I previously built.

<p align="center">
  <img src="/assets/images/banners/snake_game/part2b.png" width="600">
</p>

#### 3.	Moving the snake

Still coming from the same snake class but using different method. This method aims for the body of the snake to follow its head. Remember that snake is made of two different parts of turtle blocks and each of block could move independently.

<p align="center">
  <img src="/assets/images/banners/snake_game/part3.png" width="600">
</p>

#### 4.	Controlling the snake using keypress

This step serves for controlling purpose using keyboard up, down, left and right.

<p align="center">
  <img src="/assets/images/banners/snake_game/part4a.png" width="600">
</p>

<p align="center">
  <img src="/assets/images/banners/snake_game/part4b.png" width="600">
</p>

#### 5.	Tracking the score

I made separate a file to note and track the score (‘scoreboard.py’).

<p align="center">
  <img src="/assets/images/banners/snake_game/part5.png" width="600">
</p>

#### 6.	Feeding on the food

I also made ‘food.py’. file to write food class. I played a bit with shape and color of the food. It has turtle shape (I believe a real snake will have hard time when it tries to feed on turtle :)) and its color varies following the list of color specified at the top of the food.py file.

<p align="center">
  <img src="/assets/images/banners/snake_game/part6a.png" width="600">
</p>

<p align="center">
  <img src="/assets/images/banners/snake_game/part6b.png" width="600">
</p>

<p align="center">
  <img src="/assets/images/banners/snake_game/part6c.png" width="600">
</p>

#### 7.	Detecting collision with the wall

<p align="center">
  <img src="/assets/images/banners/snake_game/part7a.png" width="600">
</p>

<p align="center">
  <img src="/assets/images/banners/snake_game/part7b.png" width="600">
</p>

#### 8. Detecting collision with its own body

<p align="center">
  <img src="/assets/images/banners/snake_game/part8.png" width="600">
</p>

#### 9. Adding difficulty levels

This is an additional feature I added at the beginning of the game, but the last step I took. It can be achieved by changing time.sleep after importing time library. Easy was set to be 1s, medium 0.5s and hard was 0.1s.

<p align="center">
  <img src="/assets/images/banners/snake_game/part9a.png" width="600">
</p>

<p align="center">
  <img src="/assets/images/banners/snake_game/part9b.png" width="600">
</p>

### **Playing the game**

Here is the final look of the game. I have fun experimenting and trying it myself! :)

<p align="center">
  <img src="/assets/images/banners/snake_game/game_a.png" width="600">
</p>

<p align="center">
  <img src="/assets/images/banners/snake_game/game_b.png" width="600">
</p>

Find the code in [Github](https://github.com/nuki-susanti/Snake-Game)\
Connect with me! [Linkedin](https://www.linkedin.com/in/nuki-susanti-915594151/)