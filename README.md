# models

Repository containing models for our senior design project on energy system
modeling. These notebooks were originally developed on Google Colab.

This project was, in part, an effort to reproduce the results of the following
paper:

Y. Chen, Y. Shi,  and B. Zhang, "Optimal Control Via Neural Networks: A Convex
Approach," in *Proceedings of International Conference on Learning
Representations*, Apr. 30, 2018, Vancouver Convention Center, Vancouver, BC,
Canada [online]. Available: ArXiv, https://arxiv.org. [Accessed 1 Mar. 2019].

## Prerequisites

- Python 3.6+
- scikit-learn and dependencies
- TensorFlow (with keras)
- our eplusparser package, which reads the dataset

## Data generation

We used the EnergyPlus large office building in Chicago and a 11 year
weather dataset fetched from DarkSky. The `eplusparser` repository contains a
Python module for parsing and feature selection. We also provide our scripts
for fetching weather data and converting it to EPW format using PyEPW.

### Installing EnergyPlus

I installed the Arch Linux [AUR
package](https://aur.archlinux.org/packages/energyplus/).
This installs EnergyPlus to `/opt/EnergyPlus-9-0-1/`. You could probably
install the deb package or Windows package instead.

### Important notes for using EnergyPlus

- `epw`, `idf`, `idd`, etc. input files to EnergyPlus are all *plain-text*, so
  you can edit them directly with your favorite text editor.
- Not everything is reported by default in an EnergyPlus simulation.
- The EnergyPlus binary is at `energyplus`.
- Example `epw` weather files are in `WeatherData/`.
- Example building (`idf`) files are in `ExampleFiles/`. Files starting with
  `Ref` are DOE reference buildings.
  
### Typical setup

- Make a new directory somewhere
- Copy the `idf` and `epw` files you want to use
- Modify `idf` or `epw` files as needed
- Run EnergyPlus, e.g.

```bash
energyplus -a -r -w weatherfile.epw mybuilding.idf
```

### How do I run a simulation for multiple years?

- Modify the `RunPeriod` object in the `idf` file to add start and end years

```
  RunPeriod,
    ,                        !- Name 
    1,                       !- Begin Month
    1,                       !- Begin Day of Month
    2017,                    !- Begin Year <- add this
    12,                      !- End Month
    31,                      !- End Day of Month
    2019,                    !- End Year <- add this
    Sunday,                  !- Day of Week for Start Day
    No,                      !- Use Weather File Holidays and Special Days 
    No,                      !- Use Weather File Daylight Saving Period
    No,                      !- Apply Weekend Holiday Rule 
    Yes,                     !- Use Weather File Rain Indicators
    Yes;                     !- Use Weather File Snow Indicators
```

- Add `-a` to the command line to force annual simulation

### How do I add more output variables to the data?

- Run the simulation once, and you will find a `eplusout.rdd` file generated.
- Inside `eplusout.rdd` are all possible variables that can be reported.
- Search for `Output:Variable` in the `idf` file.
- You can copy your custom variables from the `rdd` file to the `idf` file
  there.  (just copy the entire line from the `rdd` verbatim)
- Run with `-r`? (Don't know if this is necessary)

## Running the models

You might have to modify the model notebooks to run on your computer. You will
definitely have to change the dataset file name (the `eplusout.sql` file) to
the name of the dataset you generated with EnergyPlus, at a minimum.
