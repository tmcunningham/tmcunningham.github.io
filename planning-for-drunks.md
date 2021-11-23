---
Title: Planning for drunks
---
# Planning for Drunks Agent Based Model

## **[View this project on GitHub](https://github.com/tmcunningham/planning-for-drunks)**

This project simulates drunks leaving a pub and moving randomly around an environment until they find their home. There are 25 drunks and each has an assigned house.

## Significant improvements

Aside from the basic assignment requirements (to have the drunks move randomly until they found their home), I made two major additions to the programme:

### 1. Drunks will sober up and eventually remember where their home is

Drunks have a ```drunk_level``` attribute, which determines "how drunk" they are. The drunk level decreases when they visit locations they have previously been to (the idea being that recognising a place helps them to sober up). When the ```drunk_level``` drops to a certain level, the drunk's speed increases. When the drunk completely sobers up (i.e. ```drunk_level = 0```), then the drunk will not move randomly and instead move in the direction of their house.

### 2. Drunks cannot move through buildings

To make the simulation more realistic, I stopped the drunks being able to walk through buildings. Instead, when they are at the edge of a building, they will randomly move along it.
