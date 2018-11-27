# CarND Model Predictive Control
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

Self-Driving Car Engineer Nanodegree Program

---

This repo is built on the starter code in Clojure provided [here](https://github.com/ericlavigne/CarND-MPC-Clojure).

Clojure version of Udacity's MPC project from term 2 of the self-driving
car engineer nanodegree. This repository is intended to serve as starter code for
other students who wish to complete the project in Clojure.

## Why Clojure?

The most common choices for self-driving car development are C++ and Python.
[Clojure](https://clojure.org/)
supports a faster development style than either of these languages (especially C++).
Compared to C++, Clojure has a much simpler and more flexible syntax, clear
error handling, and sophisticated dependency management. Compared to Python, Clojure is
much faster (close to C++) and has excellent concurrency support.

Here's a [tutorial](https://www.maria.cloud/) to help you get started.

## Installation

You will neeed to install
[Leiningen](https://leiningen.org/),
a Clojure build tool.

I also recommend [VS Code](https://code.visualstudio.com/) with the
[Calva extension](https://marketplace.visualstudio.com/items?itemName=cospaia.clojure4vscode)
as your first Clojure text editor because it is very easy to install
and use. Later, you can explore more advanced options like
[Cursive (IntelliJ)](https://cursive-ide.com/),
[CIDER (Emacs)](https://github.com/clojure-emacs/cider),
or [Vim](https://github.com/tpope/vim-fireplace).

## Usage
You can run the code with the following command. You should also run
[Udacity's term 2 simulator](https://github.com/udacity/self-driving-car-sim/releases)
at the same time and select the "MPC Control" project.

    $ lein run

## Solution

Model Predictive Control uses a cost function to determine the value of state.
Then minimizes this cost function over several discrete time steps to compute the optimal trajectory.
The first step of this trajectory is used as actuation and sent to the controller.
The process is then repeated for the next time step.

Parameters have been tuned by hand with a trial-and-error approach. Chosen number of timesteps is 11.

Positive rewards have been chosen for staying on the road and moving forward (in comparison to the road), proportional to the speed.

Penalties are applied for driving off the road (static), 
as well as distance from the center (proportional to the distance) and moving sideways (in comparison to the road, proportional to speed).

Corresponding Clojure code below:

    (+ (* vs 1.0) ; Increase result based on how quickly car is moving forward.
       (if on-road
         10.0  ; Choose bonus for staying on road
         -1000.0) ; Subtract penalty for driving off the road
       (* -15.0 distance-from-center)    ; Subtract penalty for distance from center of road
       (* vd 0.2)))) ; penalty for sideways
