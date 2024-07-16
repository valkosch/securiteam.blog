+++
title = '::Myprojects::'
date = 2024-07-13T20:48:10+02:00
draft = true
+++
Here you can find all my noteworthy works and pet projects that I have done over the years. These were either done self-taught or within my studies in my free time.

## Fractal tree and other small programming tasks
### circa 2022

I started programming to get familiar with different languages and to fulfill my creative desires. One result of this period is this fractal tree generator, which was **very** inefficient, but at least I learned about the usefulness of rotation matrices early on :)).

![Fraktálfa1](/fraktalfa1.png)
![Fraktálfa2](/fraktalfa2.png)

## Arduino Irrigation System

### November 2022

I wanted to create something practical and somewhat useful, so I made an irrigation system using a soil moisture sensor, an Arduino Uno, a relay, and a small 5V water pump. The logic behind it is very simple: the Arduino polls the sensor's value at certain intervals, maps it to a 0-100 scale, and if the value falls below 50, the irrigation process starts. I had planned to play music for the plants using a small speaker and an SD card, as certain (classical and jazz) music frequencies open the pores of plants, allowing them to receive more oxygen. It would have looked funky.

![Locsolo](/locsolo1.jpg)

## Solar-Powered Night Light
### May 2023

The basic concept is that the solar panel charges a lithium-ion battery during the day, and at night, the LED operates from this battery. I think it turned out nicely since I didn't use any light sensors, just the reverse current from the solar panel and a transistor.

![napelem1](/napelem1.jpg)
### 
Operating
![napelem2](/napelem2.jpg)

### 
Circuit Diagram

![napelem3](/napelem3.png)

## Route Planner
### September-December 2023

[Github repo](https://github.com/valkosch/A_Star.git)

![utvonaltervezo](/utvonal1.png)

This was made as the major assignment for the Programming 1 course in my first semester.

The goal of this course was to get familiar with the C language, and I feel I achieved and demonstrated that with this project. When I had to come up with what to create, I wanted to make something practical and useful in real life, not just a cute phonebook or something similar.

This program is a route planner for a small rectangle of Budapest, using the A* algorithm to find the shortest or fastest route between two points. Of course, it doesn't take into account cars, traffic jams, or traffic in general. I wrote the entire codebase myself, only importing the SDL graphics library.

The map was obtained from OpenStreetMap using a small *makegraph.py* Python script, which fetches the map from OSM, then creates a graph from it, with the data split into two separate .txt files (set of edges and vertices).
### How to

If you want to try it out, you have two options:
1. You can try it [here](https://entercomgroup.hu/23-24/Astar/index.html) without any effort because my kind colleagues uploaded it for me with WebAssembly.
2. If the above option doesn't work, or you're just curious about the source code (unfortunately, I wasn't mastered in the beauty of inline documentation at this time yet, so the code is hardly documented, but feel free to ask any questions), you can do so with the following steps:

#### Install

First, clone the git repo.

```sh

git clone https://github.com/valkosch/A_Star.git 
```

Then, you can either run the provided binary after entering the cloned directory:

```sh

./astar
```
Or you can compile and build your own executable. To do this, first download the dependencies. On my Arch Linux, this can be done with the following command, but I'm sure you can find similar packages with your own package manager. If you use Windows, don't use Windows :)

```sh

sudo pacman -S sdl2 sdl2_gfx sdl2_image sdl2_ttf
```

After that, compile the source code in the project directory:

```sh

gcc *.c -o myexec `sdl2-config --cflags --libs` -lSDL2_gfx -lSDL2_ttf -lSDL2_image -lm
```
#### Usage

After starting the program, you need to:

1. Select 2 points on the map. You can do this with 2 separate left clicks. On successful selection, the program will indicate the selected point with a red dot.
2. Choose which weighting to use for the pathfinding algorithm on the right-side interface. The fastest route, as shown by the button color code, will be drawn in red, and the shortest route in blue. The two buttons are not mutually exclusive, both can be selected at the same time if desired.
3. If the point selection and weighting are satisfactory, you can start the route planning with the "GO" button. The desired route(s) will then be drawn on the map.

At any point during the program's operation, you can clear the selected points and drawn routes with a right-click, resetting the map to its initial state. After that, you can select new start and end points. Also, you can switch between traffic and satellite map views at any time by clicking the icon in the lower right corner of the map.

## REVIS
### December 2023

[Github repo](https://github.com/valkosch/REVIS.git)

I saw [this](https://www.youtube.com/watch?v=4bM3Gut1hIk&list=PLUyyOw61zxiJXMihb4PjYbGHEgdGxMuY3&index=2) video, which was about trying to visualize binary files because it is much easier to find patterns during reverse engineering this way than looking at a series of hexadecimal numbers. Another important takeaway was that each reverse engineering program only works on a specific file type, so there is no universal solution. Inspired by this, I tried to create my own such program in C based on the video's idea.

The concept is simple: our files consist of bytes, which correspond to 2-digit hexadecimal numbers. A byte's value is at least 0 and at most 255. We create a coordinate system where both x and y range from 0 to 255. Then we interpret the bytes in a file in pairs, where the first element is always the X coordinate, and the second is always the Y coordinate.

Based on this, my program reads the file byte by byte and stores how many times each point (i.e., byte pair) occurred in a 255x255 matrix. The maximum occurrence will be a completely white pixel, 0 will be black, and the rest will be shades of these. This way, we end up with a 255x255-sized .png image representing the reverse-engineered file. We did not exploit the file type at all, making it universally good for any file consisting of 0s and 1s, which luckily is the only kind of file we know in the world of regular computers.

Examples:
### Text

![ascii](/ascii_revis.png)

Just by looking at the image, it can be determined that this is a text file, and also that it is not in Hungarian, as it only contains ASCII characters. How do I know? The decimal values of ASCII characters range from 0 to 127, and this is also reflected in the image, as there are white pixels only in the upper quarter, i.e., from 0 to 127 on both the X and Y axes.

![nonascii](/nonASCII_revis.png)

Here's another example, with the same form but now with non-ASCII characters, as can be seen. This was Shrek's Hungarian text...

### Audio

![audio](/audio_revis.png)

This was a raw audio file (important to note that if you try it for yourself, do it with a raw file because after compression, only the structure of the compression will come back in the image). I won't spoil why it looks like this; it's worth thinking about how sound occurs in the world.
### Executable


![exec1](/astar_revis.png)
#### 
![exec2](/exec_revis.png)

Executable files look interesting. With enough background knowledge, you can even determine which architecture's almost machine code we are looking at based on the image. For example, this image is from the executable of the previously mentioned major assignment, which was made for an Intel x86 architecture. Okay, there's text in it, but what are those vertical and horizontal lines?

```asm

	mov	rdi, rbx	#, tmp333
	call	mapItNodes@PLT	#
	lea	rdi, 128[rsp]	# tmp336,
	call	GetMapI@PLT	#
	mov	QWORD PTR 96[rsp], 0	# openSet.first,
	mov	QWORD PTR 104[rsp], 0	# openSet.last,
	mov	QWORD PTR 112[rsp], 0	# closedSet.first,
	mov	QWORD PTR 120[rsp], 0	# closedSet.last,
	mov	QWORD PTR 32[rsp], 0	# StartNode,
	mov	QWORD PTR 40[rsp], 0	# EndNode,
```
Here's the solution. This is a part of the code of my major assignment, one level lower, in Intel x86 assembly code. You can notice that the MOV operator appears quite frequently, usually using different registers. The MOV and the addresses of the registers can also correspond to bytes, and in this case, we get exactly those straight lines you see.

### Images

![img1](/img_revis.png)
#### 
![img2](/img_raw_revis.png)

These might be the most spectacular. I used raw images here. The characteristic of images is that the adjacent pixels do not differ much from each other. Think of a sunset where there are no sharp transitions; instead, one color gradually changes into another. This property can be recognized here as well.

![img3](/img_stego_revis.png)

This is also worth examining, as it also came from an image, although it is visible that it is not raw rather compressed. This was generated from a .png file, but something is not right; something is suspicious. The pattern of texts is visible in it. This is because it was a .png image in which I hid some text. Such a hidden thing can be easily uncovered with this program.

## PNG Inject
### January 2024

[Github repo](https://github.com/valkosch/png_inject.git)

This was not a huge project either, but since I mentioned earlier that I hid text in an image, I might as well present this too. Around this time, I became interested in steganography, which means not only encrypting something but also hiding the fact that something is encrypted. This little program is simply capable of exploiting the png standard to insert a chunk into the png that reading programs will ignore, so the image will appear the same, but at the same time, it places an arbitrary payload into this chunk.

## Cipher
### February - May 2024

[Github repo](https://github.com/valkosch/cipher.git)

For the Programming 2 course, we also had to make a project using the new features of C++ and the paradigms of object-oriented programming. This is a framework in which some STL containers were implemented, such as String, List, and Vector, as well as some classic symmetric encryption methods like XOR, Bifid, and Vigenere, as this was the basic concept of my chosen task. Furthermore, as a challenge, I created the SHA256 hash function, which allows generating 256-bit hashes from arbitrary text. It returns the same hash digest as any other official SHA256 tool. I did this because, for example, using this, one can create an account system where passwords and usernames associated with accounts are stored in such a hash, since we **never store passwords in plaintext**. For this task, I took the importance of documentation much more seriously, which is evident in the frequency of comments, and I also used Doxygen.

If you are interested in the documentation, you can find it [here](https://github.com/valkosch/cipher/blob/main/dokumentacio_nhf.pdf).

## liftech.hu
### March - July 2024

[Github repo](https://github.com/valkosch/liftech.git)

![liftech](/liftech.png)

During the second semester, I took on the task of creating a website, for which I tried to use the latest technologies to achieve the highest quality result. Therefore, I used the Next.js framework with the app router. The purpose of the site was advertising, contact, and presentation. Since I did everything, I was able to improve both frontend and backend skills during the work. The site required a Node.js runtime environment on the server, setting up a firewall, configuring an Nginx proxy, and writing an API for the form part of the website. Overall, I think I learned a lot during its creation, while meeting my client's expectations and not failing university :).

My client's feedback as a reference:

The site: [liftech.hu](https://www.liftech.hu)

