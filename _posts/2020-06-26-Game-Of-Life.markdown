---
layout: post
title: "Conway's Game of Life"
date: 2020-06-23 00:00:00 +0300
description: # Add post description (optional)
img: conwaysgameoflife.png # Add image post (optional)
b_img: conwaysgameoflife.png # Add post image (optional)
desc: "Lambda School - Build Week 5"
tags: [iOS] # add tag
---

<!-- View on GitHub -->
<center><a class="github-button" href="https://github.com/wyattharrell/Conways-Game-of-Life" data-color-scheme="no-preference: dark; light: dark; dark: dark;" data-size="large" aria-label="Use this template wyattharrell/istats on GitHub">View on GitHub</a></center>

> The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970. It is a zero-player game, meaning that its evolution is determined by its initial state, requiring no further input. One interacts with the Game of Life by creating an initial configuration and observing how it evolves. It is Turing complete and can simulate a universal constructor or any other Turing machine.
-- <a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life" target="_blank">Conway's Game of Life, *Wikipedia*</a>

### Motivation

During my first computer science build week at Lambda School, I built Conway's Game of Life as an iOS app. The app is complete with a library of presents, the ability to play, pause, skip forward, and wipe the board. It also allows the user to tap the cells, toggling them as alive or dead. I was extremely happy with how the app turned out and I am specifically proud of the UI.

### Key-Value Observing

In order to update the current generation number and live cell population as they fluctuated, I needed to use key-value observing. I had previously used KVO in an Objective-C project, so it was interesting to see how it works in Swift. 

I first conformed the `GameBoard` class to `NSObject`. Then I added the `@objc dynamic` attributes to the generation and population variables.

{% highlight swift %}
class GameBoard: NSObject {
    // MARK: - Properties
    @objc dynamic var generation: Int
    @objc dynamic var population: Int
{% endhighlight %}

Back in the `GameOfLifeViewController`, I set up the observers to configure the labels.

{% highlight swift %}
class GameBoard: NSObject {
private var generationObserver: NSKeyValueObservation?
private var populationObserver: NSKeyValueObservation?

generationObserver = golView.gameBoard.observe(\.generation) { [weak self] object, _  in
	self?.generationLabel.text = "Generation\n\(object.generation)"
}

populationObserver = golView.gameBoard.observe(\.population) { [weak self] object, _ in
	self?.populationLabel.text = "Population\n\(object.population)"
}
{% endhighlight %}

