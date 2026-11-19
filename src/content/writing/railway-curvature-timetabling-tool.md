---
title: "The Railway Curvature & Timetabling Tool"
description: "A QGIS tool for asking \"how fast could this railway actually go?\""
date: 2026-08-01
draft: false
---

*A QGIS tool for asking "how fast could this railway actually go?"*

Suppose you have a railway: if you rebuilt this line properly, how fast would a passenger train actually run?

The honest answer is "it depends," and it depends in ways that aren't obvious until you look at the geometry curve by curve. A corridor's curvature, not its length, is usually what sets its journey time. However, you may be surprised how with sufficient banking of curves, modernized operations and good equipment how fast even a cheaply engineered railway from the 19th Century might be operated on a simple schedule.

So I built a tool to answer this properly instead of guessing, and I'm sharing it in case it's useful to anyone else poking at similar questions.

## What it does

You give it a track centreline (any line layer in QGIS) and a CSV of station locations. It:

1. Stitches the geometry into one continuous route and measures it precisely
2. Fits a spline and computes curvature analytically along its length
3. Converts that curvature into a permissible speed, using proper cant and cant-deficiency math, including the EN 13803 rules for how much distance you need to ramp cant at a reverse curve
4. Runs a train over that speed profile with real traction physics: power, tractive effort, braking, curve resistance, grades if you feed it a DEM, in both directions (note, DEM integration is slow)
5. Works out where opposing trains on a single track would meet, and therefore where passing loops need to go, without you having to draw them in first
6. Ranks every curve on the route by how many seconds it actually costs, so you can see which realignments would be worth the money and which wouldn't

The output is three QGIS layers: a speed profile, a curve inventory styled by time lost, and a set of passing siding locations, plus optional CSVs if you want to work with the numbers elsewhere.

## What it deliberately doesn't do

The run times are the geometric potential of the alignment; what's physically achievable given the curves, grades, and your assumed rolling stock. A real timetable would run slower once you add schedule complexity, station approach limits, and all the operational friction the model doesn't see. Treat it as an upper bound and a way to compare scenarios against each other, not as a schedule. By default, all schedules are padded 7%, however this figure is adjustable.

## Get it

The script is a single QGIS Python Console file, and the full code, with a detailed setup guide at the top, is on GitHub: [Railway Curvature & Timetable Generator (Gist)](https://gist.github.com/BSDawe/52c5fd15202fee9912c7391dbf8352cd).

It's licensed CC BY-NC 4.0: free to use, adapt, and share for personal, educational, or research purposes with attribution. If you'd want to use it for paid consulting work or as part of your job at a transit agency or engineering firm, just get in touch first. I'm generally happy to say yes, I just want to know it's being used.

If you run it on your own corridor of interest, I'd genuinely like to hear what you find.

<p class="note">Originally published on <a href="https://cartoview.blogspot.com/2026/08/the-railway-curvature-timetabling-tool.html">A Cartographic View</a>.</p>
