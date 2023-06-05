![Rocky Road Brownies](./../assets/images/Rocky_Road.jpeg){: style="width:2.7559055118110236in;height:1.8371686351706036in"}.  
By Chocolate-Dessert-Recipes.com - Rocky Road Brownies, CC BY 2.0, https://commons.wikimedia.org/w/index.php?curid=48410887

**[Teaching Outcomes]{.underline}**

-   Students able to list the issues relating to running software on
    users machines

-   Students can name the pros & cons of native installations, VMs and
    Containers

**[Teaching Experience(s)]{.underline}**

-   Lecture

-   Google quiz

-   Open class discussion of model programs

## 1.1 Enabling use of your software by other Researchers

So far we have learnt how to program, how to document our code, how to
test it and how to publish it to a repository (Findable & Accessible
under the FAIR principles
<https://force11.org/info/the-fair-data-principles/> ) -- what more do
we have to do?

***Why is it so difficult to run other peoples' code?***

This is a fair question -- over your career there will be many times you
will come across software that is potentially useful to you, that you
download & install and find that it doesn't work (maybe with a cryptic
error message for you to Google).

[Potential barriers:]{.underline}

-   Their computer has different hardware or operating system to yours.\
    Executable code is often created to run on a specific microprocessor
    hardware. MS-Windows programs run on ia-x86 processors (and possibly
    ARM ones) whilst MacOS has run on 68K, PowerPC, Ia86 and ARM
    (M1&M2).\
    The main target Operating systems are Windows, MacOS & Linux -- all
    of which have different interactions with hardware and
    applications.\
    In theory this can be catered for by developing versions for the
    different hardware and OS platforms nut this can be expensive in
    time, equipment, and support resources.

-   Different Software versions

-   Users' Technical ability -- It's wrong to assume high IT experience
    or to expect them to attain it just to use your software (Raising
    the 'cost of entry' and dissuading them from using it).


|  Pros    |  Cons |
|---|---|
| -   Closest to the software authors' development environment  | -   Not always easy to capture exact pre-requisites     |
|   | -   Can require significant IT experience or support if issues are encountered    |
|   | -   Could override/replace version of existing software on users PC          |

**[Challenge]{.underline}**:

> In teams analyse the example R or Python programs with respect to
> potential barriers to usage by others.\
> We will then have a class discussion of the potential issues you have
> identified

***So, what can we do about this?***

-   Extensive documentation on installing all the pre-requisites to run
    our software including specific version numbers. Note versions are
    prone to change and so we would have to keep this 'recipe' up to
    date to ensure continued success.

-   Create a Virtual Machine (VM)

-   Create a software Container e.g. a Docker container.

## 1.2 What is a Virtual Machine?

Stated simply a virtual machine is software that emulates the hardware
and software of a computer and runs this emulation on your (host)
computer. A layer, called the hypervisor, has the role of running the VM
and transferring data in and out of it.

![hypervisor host os of a virtual machine diagram
](./../assets/images/virtual.jpg){: style="width:3.1in;height:3.1in"}. 

-   Examples of software to host VMs Microsoft Hyper-V, Virtualbox (Mac
    OS, Windows & Linux)

| Pros  | Cons  |
|---|---|
| -   Does not affect Host OS | -   Can require a similar level of IT knowledge/expertise to create the software  environment for VM |
| -   Can run several servers on one machine | -   Each VM simulates the client OS as well as the hardware so they can get large and heavily consume disk & memory resources and can be slow to start up      |
| -   Simulates hardware resources that may not be present on host hardware |   |


## 1.3 What is a Container?

Let's consider the analogy of shipping containers that are used
throughout the world.

![A large orange truck drives down the road Description automatically
generated with medium
confidence](./../assets/images/container_lorry.jpg){: style="width:2.23in;height:2.8in"}. 

Photo by [Nur
Alamin](https://unsplash.com/@nuralamin12?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/xifUN_Mkf8Y?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

![A picture containing sky, outdoor, outdoor object Description
automatically generated](./../assets/images/container_ship2.jpg){: style="width:2.8in;height:1.8in"}. 

Photo by [Ian
Taylor](https://unsplash.com/@carrier_lost?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/jOqJbvo1P9g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

![](./../assets/images/container_train.jpg){: style="width:2.76in;height:1.8in"}. 

Photo by [Michael
SKOPAL](https://unsplash.com/@michael_skopal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/z5tiShyxZnc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

![](./../assets/images/container_ship.jpg){: style="width:2.1in;height:3.15in"}. 

Photo by [Nathan
Cima](https://unsplash.com/@nathan_cima?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/MHXJ9p64Jw8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**Real-world containers are:**

-   **Standardised size/shape/Dimensions**

-   **Stackable design**

-   **Carried by a variety of means of transportation**

-   **Standardised machinery to load & unload containers**

A software container is defined by a manifest or recipe file that lists
all of the software and files to be installed within it but makes use of
functions from the Host OS,

-   **Can be executed on Windows, MacOS & Linux**

-   **Standardised Recipe for building and loading/running a container
    for all supported platforms**

-   **Multiple containers can be run on a Host OS**

![container elements diagram](./../assets/images/container.jpg){: style="width:4.6in;height:2.5in"}. 


| Pros  | Cons  |
|---|---|
| -   Does not affect software installations on host computer i.e. install into *tabula rasa* environment. | -   Requires expertise/training in creating the Dockerfiles\* |
| -   Lightweight -- consumes less resources on host computer and faster to launch than VMs | -   Should do tests to compare results with native install |
| -   Uses Dockerfiles\* to script construction of compute environment |   |
| -   Supported/welcomed by most Cloud computing providers. Many click & launch options(including [Gitpod](https://www.gitpod.io)) are available.   |   |

\* We will cover creating a Dockerfile later in this course

**[Challenge]{.underline}**:
> Go to this Google quiz about the material we have covered in this
> introduction {link\] and answer the questions (Your answers are
> anonymised)
> 
> We will then have a review of the class answers and address any
> misunderstandings highlighted by this exercise.




