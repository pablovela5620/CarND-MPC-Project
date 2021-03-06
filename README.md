# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---
Video of Model Predictive Controller in action

[![Project Video](https://img.youtube.com/vi/OSmgWdpi8_0/0.jpg)](https://www.youtube.com/watch?v=OSmgWdpi8_0&feature=youtu.be)


## Model Description and Details
Below are the model's actuation, constraint, cost functions.

#### Model

![Model_1](https://latex.codecogs.com/gif.latex?x_%7Bt&plus;1%7D%3Dx_%7Bt%7D&plus;v_%7Bt%7D%20%5Cast%20%5Ccos%20%5Cleft%20%28%20%5Cpsi%20_%7Bt%7D%20%5Cright%20%29%5Cast%20dt)

![Model_2](https://latex.codecogs.com/gif.latex?y_%7Bt&plus;1%7D%3Dy_%7Bt%7D&plus;v_%7Bt%7D%20%5Cast%20%5Csin%20%5Cleft%20%28%20%5Cpsi%20_%7Bt%7D%20%5Cright%20%29%5Cast%20dt)

![Model_3](https://latex.codecogs.com/gif.latex?%5Cpsi_%7Bt&plus;1%7D%3D%5Cpsi_%7Bt%7D&plus;%5Cfrac%7Bv_%7Bt%7D%7D%7BL_%7Bf%7D%7D%5Cast%20%5Cdelta%20_%7Bt%7D%5Cast%20dt)

![Model_4](https://latex.codecogs.com/gif.latex?v_%7Bt&plus;1%7D%3Dv_%7Bt%7D&plus;a_%7Bt%7D%20%5Cast%20dt)

![Model_5](https://latex.codecogs.com/gif.latex?cte_%7Bt&plus;1%7D%3Df%5Cleft%20%28%20x_%7Bt%7D%20%5Cright%20%29-y_%7Bt%7D&plus;v_%7Bt%7D%20%5Cast%20%5Csin%5Cleft%20%28%20e%5Cpsi%20_%7Bt%7D%20%5Cright%20%29%5Cast%20dt)

![Model_6](https://latex.codecogs.com/gif.latex?e%5Cpsi_%7Bt&plus;1%7D%20%3D%20%5Cpsi%20-%20%5Cpsi%20des_%7Bt&plus;1%7D%20&plus;%20%5Cfrac%7Bv_%7Bt%7D%7D%7BL_%7Bf%7D%7D%5Cast%20%5Cdelta%20_%7Bt%7D%5Cast%20dt)

#### Constraints

![Constraints_1](https://latex.codecogs.com/gif.latex?%5Cdelta%20%5Cepsilon%20%5Cleft%20%5B-25%5E%7B%5Ccirc%7D%2C%2025%5E%7B%5Ccirc%7D%5Cright%20%5D)

![Constraints_2](https://latex.codecogs.com/gif.latex?a%20%5C%3A%20%5Cepsilon%20%5Cleft%20%5B-1%2C%201%5Cright%20%5D)

#### Cost

![Cost](https://latex.codecogs.com/gif.latex?J%20%3D%20%5Csum_%7Bt%3D1%7D%5E%7BN%7D%5Cleft%20%28%20cte_%7Bt%7D%20-%20cte_ref%20%5Cright%20%29%5E%7B2%7D&plus;%5Cleft%20%28%20e%5Cpsi%20_%7Bt%7D%20-%20e%5Cpsi%20ref%5Cright%20%29%5E%7B2%7D%20&plus;...)

## Timestep Length and Elapsed Duration

For this project I have chosen N = 10 (timestep length) and dt = 0.15 (elapsed duration between time steps). The choice of N and dt is based around
the duration the optimizer considers. So with these chosen values the optimizer is looking at a total time duration of 1.5 seconds. I had previously
chosen values of N = 10 and dt = .10 but this would cause the car to veer of course at high speeds because it didnt look far enough into the chosen path

## MPC Preprocessing and Latency

To simplify the controller before feeding in points into the solver, the x and y as well as the cars psi angle were shifted to its own coordinate system so 
that the polyfit equation would have an easier time, such as avoiding having to fit to a potentially vertical function and having
the position be at the origin (0,0). The simulator allows for latency to be added to the problem thus in this case a latency of 100ms was used.
This was dealt with by changing what the cars x, y, psi, and v where to what they should be 100ms in the future (lines 107- 111)


## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.)
4.  Tips for setting up your environment are available [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
5. **VM Latency:** Some students have reported differences in behavior using VM's ostensibly a result of latency.  Please let us know if issues arise as a result of a VM environment.

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/b1ff3be0-c904-438e-aad3-2b5379f0e0c3/concepts/1a2255a0-e23c-44cf-8d41-39b8a3c8264a)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## References
[Udacity MPC Controller Q&A](https://www.youtube.com/watch?v=bOQuhpz3YfU&list=PLAwxTw4SYaPnfR7TzRZN-uxlxGbqxhtm2&t=2528s&index=5)