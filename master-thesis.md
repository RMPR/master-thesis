---
title: "Master thesis"
subtitle: ""
author: ["akoudanilo@gmail.com", "OSID: 15P080"]
date: "2019-11-04"
subject: "Markdown"
keywords: [CI/CD, Devops, capacity planning, emsit]
subtitle: "Dimensioning and setting up a cloud infrastructure for the deployment of a SaaS solution: case of EMSIT"
lof: true
lang: "en"
titlepage: true
titlepage-color: "1E90FF"
titlepage-text-color: "FFFAFA"
titlepage-rule-color: "FFFAFA"
titlepage-rule-height: 2
book: true
classoption: oneside
code-block-font-size: \scriptsize
bibliography: project.bib
csl: mla.csl
margin-left: 70px
margin-right: 55px
margin-bottom: 100px
---

# Preamble

## Acknowledgement
I would like to express my deepest appreciation to all those who provided me the possibility to complete this report.
First thing first my parents, without who I wouldn't even exist in the first
place and logically God Who makes all things possible.
A special gratitude goes to my mentor, Mr Aurelien Aptel for his time taken to explain things to me, for cleaning up and correcting errors that I might have missed otherwise.
Furthermore I would also like to acknowledge with much appreciation the crucial role of my teachers, especially Mr KOUAMOU Georges, who gave me the basics in algorithms needed to achieve my work, specifically the algorithms I had to develop during this internship.
A special thanks goes to my family, friends and classmates who helped me in their own ways by providing support, empathy and courage to go through all that.

## Sigles et abbreviations
- **AWS**: Amazon Web Services
- **GCP**: Google Cloud Platform
- **CI**: Continuous Integration
- **CD**: Continuous Delivery

## Abstract
CI/CD is a means of implementing agile development principles across the 
software testing and delivery process, which thereby makes software development 
more nearly interactive as well. CI/CD allows for nearly constant updates to 
software, making the whole process highly responsive to business requirements 
and user needs. This intersect directly with the capacity planning part, because
quick feedback implies load increase and load increase means dimenionning servers
accordingly.


## Résumé
L'approche CI/CD permet d'augmenter la fréquence de distribution des applications grâce à l'introduction de l'automatisation au niveau des étapes de développement des applications. Les principaux concepts liés à l'approche CI/CD sont l'intégration continue, la distribution continue et le déploiement continu. L'approche CI/CD représente une solution aux problèmes posés par l'intégration de nouveaux segments de code pour les équipes de développement et d'exploitation (ce qu'on appelle en anglais « integration hell », ou l'enfer de l'intégration).

Plus précisément, l'approche CI/CD garantit une automatisation et une surveillance continues tout au long du cycle de vie des applications, des phases d'intégration et de test jusqu'à la distribution et au déploiement. Ensemble, ces pratiques sont souvent désignées par l'expression « pipeline CI/CD » et elles reposent sur une collaboration agile entre les équipes de développement et d'exploitation.




## General Introduction
A CI/CD pipeline is one of the best practice to date for Devops teams to implement,
for delivering code changes more frequently and reliably, dimensionning is tighly
coupled to the act of capacity planning which is in turn tighly coupled with preformance,
or how do we need to adjust our habits for this to work correctly? To do any sort 
of capacity planning, you have to measure and monitor performance.

## Presentation of Digital House International

![DHI Logo](./img/DHI-logo.png){width="50%"}

### Overview
Digital House International (DHI) is a tech company with some expertise in 
applications development ranging from Web to mobile apps with a focus on quality.

### How it works
The services are oriented towards 3 main activities:

- Audit and advice

According to the needs of the client, the company provides support and advices 
on technical, ux / design and web marketing choices.

- Creation and design

The client image is important and central to optimize the conversion of their visitors 
into customers. This is why it must be treated in a coherent and thoughtful manner. 
The Creation & Design department will work closely with the client to create creative concepts 
that meet their needs and set them apart from the competition.

The department designs ergonomic interfaces for attractive mobile sites and applications

- Architecture and development

A web or mobile application cannot exist without the server infrastructure and 
the services associated with it. The company's experts design scalable, robust and secure 
architectures to guarantee the availability of your application in the cloud.

The architecture and development department supports the client in defining the functional 
area and the technical scope of their future application.

### What is DHI?

- In France

DHI is a SAS with a capital of 1034 euros, registered with the RCS of Lille under 
the number SIRET 840 492 862 00015, and having the code NAF or APE 6201Z.
Reachable at the following address, 165 Avenue de Bretagne 59000 Lille France
(Headquaters).

- In Cameroon

DHI is a SARL with a capital of 900,000 FCFA, registered in the Douala commercial 
register under number RC / DLA / 2019 / B / 769. Reachable at 355 Rue Alfred Saker - 
AKWA BP 2858 Douala Cameroon.

The person in charge of the publication is Dr. Césaire FOTSING KWETCHE, which happens 
to also be the manager of the company, reachable by phone at (+237) 6 96 40 40 62

# State of the art of CI/CD Tools

## Introduction
Let's say we have a network of computers with active directory configured, for
one reason or another, when we try to enable one feature of active directory 
the connectivity seems to have performance issues. The issues can be that 
the network is either failing or too
slow and we have two capture files obtained using any capture tool (wireshark,
tcpdump, ...) of the two states of the network. One question we can ask is
how can we 
analyze what changed in the network between the activation of the feature
and when everything worked perfectly ?

### Problematic

We want to deliver changes to end users more frequently (including bug fixes) with
minimal risk regarding which version of the app is currently running in production,
and anticipate the cost (in terms of hardware) we will be facing.

## Classical approach: Deploy everything manually

&nbsp; Since the dawn of time, companies had always documented processes, because
an undocumented process is lost. Hence a lot of paper to indicate to the future 
deployer what to do and workarounds in case of problems, in this typical case,
the time from the development to the deployement (including feedback about deployment)
is very long (measured in months).


## Middle approach: Scripting the process

A good thing about servers is: they generally run using Unix like systems: Linux,
solaris, *BSD, ... which are easily scriptable, this makes the deployment process
as simple as executing a script with a given set of parameters and following through
the procedure, with human intervention only when necessary, this is better than the
previous one fully manual but there's one problem which arise quickly: maintenability
of scripts when the deployement procedure become more and more complex, beside the fact
that scripting languages (bash in this case to avoid unuseful dependencies) aren't
really well known for the readability ( in fact they are always used for quick and dirty
hacks), arise the burden of having to maintain 2 codebases, the first one is the 
code running in production, and the second one is the deployment code base (you 
can reach the point where you need a deployment script for your deployment script :) ).

## Approach adopted: pipeline CI/CD

In simple words, a CI/CD pipeline is a black box taking code as an input and output
a modification in the software running in production (the modification can be no 
modifications at all for reasons explained later).

Formally: [Find a mathematical expression of CI/CD]

Knowing the advantages and the disadvantages of the two previous approachs, 
we tried to address all of those problems using this 
The technical goal of CI is to establish a consistent and automated way to build, 
package, and test applications. With consistency in the integration process in 
place, teams are more likely to commit code changes more frequently, which leads 
to better collaboration and software quality.

Continuous delivery picks up where continuous integration ends. CD automates the 
delivery of applications to selected infrastructure environments. Most teams work 
with multiple environments other than the production, such as development and 
testing environments, and CD ensures there is an automated way to push code 
changes to them.

CI/CD tools help store the environment-specific parameters that must be packaged 
with each delivery. CI/CD automation then performs any necessary service calls 
to web servers, databases, and other services that may need to be restarted or 
follow other procedures when applications are deployed.

### Text
Each line is a packet and sub-packets are indented by one tab.
We can obtain it with the command:

```shell
tshark -2 -R "smb or smb2" -r /path/of/capture/file
```

![Textual output Wireshark](./img/tshark_std_output.png)


### Json
The json file is an array of objects and each object is a packet, each
packet contains many key/value pairs corresponding to a field or a subfield 
of the protocol used (header, flags, length, ...).
We can obtain it with the command:
```shell
tshark -2 -R "smb or smb2" -r /path/of/capture/file -T json
```

![Json output Wireshark](./img/tshark_json_output.png){width="70%"}

\pagebreak


### XML (PDML)
A PDML file contains multiple packets, denoted by the `<packet>` tag. A packet will contain multiple protocols, denoted by the `<proto>` tag.
A protocol might contain one or more fields, denoted by the `<field>` tag. 
We can obtain it with the command:

```shell
tshark -2 -R "smb or smb2" -r /path/of/capture/file -T PDML
```

![XML output Wireshark](./img/tshark_xml_output.jpg)

\pagebreak


# Design of the solution

## Analysis
In this section, we discuss about the stakeholders of our system, the different
actors, the functional and non-functional requirements and finally establish the
use cases and class diagram.

### Requirements engineering

#### Functionnal requirements
- visualize diffs side by side
- permute packets: Usually diffs are from packet A to packet B, if the user 
wants from B to A, he must not reload the packets
- Change different settings: colors, diff method
- search inside the packets

#### Non-functional requirements
- Use the programming language python
- Must use external dependencies only if necessary, moreover, the program must
work standalone
- No databases, store everything in files

### Use case diagram (GUI)

![Use case diagram smbcmp-gui](./img/use_case_diagram_smbcmp.png){width="80%"}

\pagebreak


- **Stakeholders**
The main stakeholder here are the people inside the company because it's an 
internal tool open sourced.

- **Actors**

- Main actor
  - User
- Secondary actor
  - smbcmp

- **Use cases**
- Open files : load the files inside the buffers
- Permute buffers : exchange files
- Change settings : like colors, diff engine
- Search packets : search packet in each buffer

### Class diagram 
We have the following class diagram:

![Class diagram](./img/class_diagram.png)

\pagebreak

## Technologies used 

### For the command line interface
The objective was to have as little dependencies as possible so in this part, 
there is only an *optionnal* dependency: `lxml` one may ask why, if it's 
optionnal
it can also be removed without many harm but in our research, lxml has been 
proven to be quicker than `cElementTree` (which is the standard library to 
parse xml files) for the operation we had to do on
the XML tree (mainly serialising and parsing)  as a side note, the two 
libraries propose the 
same API thus reducing the overhead of using them concurrently.

The following subsections are about the benchmark:
[@BenchmarksSpeed]

- **Serialising**

For serialising, lxml is more than 10 times as fast as the much improved 
ElementTree 1.3
in recent Python versions:
```shell
lxe: tostring_utf16  (S-TR T1)    7.9958 msec/pass
cET: tostring_utf16  (S-TR T1)   83.1358 msec/pass

lxe: tostring_utf16  (UATR T1)    8.3222 msec/pass
cET: tostring_utf16  (UATR T1)   84.4688 msec/pass

lxe: tostring_utf16  (S-TR T2)    8.2297 msec/pass
cET: tostring_utf16  (S-TR T2)   87.3415 msec/pass

lxe: tostring_utf8   (S-TR T2)    6.5677 msec/pass
cET: tostring_utf8   (S-TR T2)   76.2064 msec/pass

lxe: tostring_utf8   (U-TR T3)    1.1952 msec/pass
cET: tostring_utf8   (U-TR T3)   22.0058 msec/pass
```
- **Parsing**

For parsing, lxml.etree and cElementTree compete for the medal. Depending on the input, either of the two can be faster. The (c)ET libraries use a very thin layer on top of the expat parser, which is known to be very fast. Here are some timings from the benchmarking suite:

```shell
lxe: parse_bytesIO   (SAXR T1)   13.0246 msec/pass
cET: parse_bytesIO   (SAXR T1)    8.2929 msec/pass

lxe: parse_bytesIO   (S-XR T3)    1.3542 msec/pass
cET: parse_bytesIO   (S-XR T3)    2.4023 msec/pass

lxe: parse_bytesIO   (UAXR T3)    7.5610 msec/pass
cET: parse_bytesIO   (UAXR T3)   11.2455 msec/pass
```
- **Child Access**

The same tree overhead makes operations like collecting children as in list(element) more costly in lxml. Where cET can quickly create a shallow copy of their list of children, lxml has to create a Python object for each child and collect them in a list:

```shell
lxe: root_list_children        (--TR T1)    0.0038 msec/pass
cET: root_list_children        (--TR T1)    0.0010 msec/pass

lxe: root_list_children        (--TR T2)    0.0455 msec/pass
cET: root_list_children        (--TR T2)    0.0050 msec/pass
```

- **Element creation**

As opposed to ET, libxml2 has a notion of documents that each element must be in. This results in a major performance difference for creating independent Elements that end up in independently created documents:

```shell
lxe: create_elements           (--TC T2)    1.0045 msec/pass
cET: create_elements           (--TC T2)    0.0753 msec/pass
```

- **Merging different sources**

A critical action for lxml is moving elements between document contexts. It requires lxml to do recursive adaptations throughout the moved tree structure.

The following benchmark appends all root children of the second tree to the root of the first tree:
```shell
lxe: append_from_document      (--TR T1,T2)    1.0812 msec/pass
cET: append_from_document      (--TR T1,T2)    0.1104 msec/pass

lxe: append_from_document      (--TR T3,T4)    0.0155 msec/pass
cET: append_from_document      (--TR T3,T4)    0.0060 msec/pass
```

- **Deepcopy**
  
Deep copying a tree is fast in lxml:

```shell
lxe: deepcopy_all              (--TR T1)    3.1650 msec/pass
cET: deepcopy_all              (--TR T1)   53.9973 msec/pass

lxe: deepcopy_all              (-ATR T2)    3.7365 msec/pass
cET: deepcopy_all              (-ATR T2)   61.6267 msec/pass

lxe: deepcopy_all              (S-TR T3)    0.7913 msec/pass
cET: deepcopy_all              (S-TR T3)   13.6220 msec/pass
```

- **Tree traversal**

Another important area in XML processing is iteration for tree traversal which
is especially useful if the algorithms can benefit from step-by-step traversal 
of the XML tree bonus points if few elements are of interest or the target element tag name is known.

```shell
lxe: iter_all             (--TR T1)    1.0529 msec/pass
cET: iter_all             (--TR T1)    0.2635 msec/pass

lxe: iter_islice          (--TR T2)    0.0110 msec/pass
cET: iter_islice          (--TR T2)    0.0050 msec/pass

lxe: iter_tag             (--TR T2)    0.0079 msec/pass
cET: iter_tag             (--TR T2)    0.0112 msec/pass

lxe: iter_tag_all         (--TR T2)    0.1822 msec/pass
cET: iter_tag_all         (--TR T2)    0.5343 msec/pass
```

From this benchmark, it's clear that lxml outperforms blatantly cET for our
use cases and it even propose methods not implemented yet in cET, like a xpath
engine but for practical reasons (keep the dependecy optionnal) we only used
common features.

### Graphical User Interface

In the Python GUI multiplatform programming ecosystem, there are a few
legit choices (mature enough to be considered):

- **WXpython**: Python bindings of the WXWidgets framework 
- **Tkinter**: Standard library for GUI programming
- **Pyside 2 (Qt for python)**: Python bindings for the QT framework
- **PyQT**
- **Kivy**
- **PyGTK**
- **PySimpleGUI**

There is a comparative table for all of them based on the following criteria:

Framework | License | Documentation | Modern UI | Wysiwyg | Target | Native
----------|----------|--------------|-----------|---------|-----|-----
 Wxpython | GPL v3 | Good | Yes | Yes | Desktop | Yes
 Tkinter | GPL v3 | Good | No | No | Desktop | No
 Pyside 2 | GPL v3 | Poor | Yes | Yes | Desktop | No
 PyQT | Commercial | Medium | Yes | Yes | Desktop | No
 Kivy | BSD | Good | Yes | No | Mobile | No
 PyGTK | GPL v3 | Medium | No | Yes | Desktop | Only on Gnome
 PySimpleGUI | GPL v3 | Medium | Yes | No | Desktop | Yes

Because fo the requirements and the documentation, we ended up choosing 
*WXpython* for the project.


# Capacity Planning
I think a lot of system administrators either fear capacity planning or just think it's unnecessary. First, there's no reason to fear capacity planning (it isn't rocket science); and second, capacity planning is 100% necessary. In the past, system administrators had to deal with management making sweeping decisions to add capacity and enhance performance, either by throwing new systems into the mix or adding CPU, RAM, or faster storage. Usually, but not always, the problem persisted beyond the upgrades and added capacity. But the "usually" qualifier is the part of the equation that stumps system administrators and managers alike—to the point that no one wants to deal with actual capacity and performance planning and administration.

## First: Get a baseline
It doesn't matter whether your systems are brand new or three years old, you must establish a baseline before you can begin capacity planning and projection. Establishing a baseline is somewhat time-consuming because a baseline is not a snapshot, it is rather a longer-term view of performance. Use at least a one-month baseline for each system. A month of data should give you the range of performance from which you can plan and forecast capacity needs.

## Second: Set up performance monitoring
TL;DR sysstat

# Implementation
For this part, we had to implement a few algorithms, here we hightlight 2 of
those which are recursive:

## Algorithm 1: recursive diffs between 2 nodes of the XML graph

### Principle 
We have two top-level packets `A` and `B`, we face the following cases:

- **Cases where A and B have different numbers of children**

In this case we identify each child, match those which are the same and 
mark the remaining as either removed or added depending on which the parent,
if it's `A` we mark added and if it's `B` we mark removed.

- **1 Leaf node vs 1 Folder node**

If not child_a.children or not child_b.children:
In this case we just keep going deeper inside the folder node, and 
we print all that as diffs.

- **Terminal cases: 2 leaf nodes**

If the nodes are roughly the same (just the values changed) we store this 
change else we mark as two different lines.

- **Recursive case: 2 Tree nodes**
 
We call the function again on the two nodes.

### Pseudo-code
The pseudo-code is quite long, you can find it in [Appendix B](#appendix-b---recursive-PDML-diffs-algorithm)

### Complexity analysis 
We make a few assumptions, the packets `A` and `B` which are represented
as trees have respectively n and m elements 

#### Time complexity
In the worst case, the packets aren't the same but have the same structure with
the same number of nodes, in this case, we have to go to each leaf just to
notice that the leafs aren't the same and marked the first one as added and the
second one as removed. We have obviously O(n^2)


#### Space complexity
We store the both trees and use a constant space to make diffs, so O(n^2).

## Algorithm 2: Circular search inside a non-circular list

### Principle

We have a list of strings (packets) `data` inside a `buffer` and we are looking
for a certain string `text` and if we find it, (even a partial match) we update the `buffer` selected item and stop the search.

In order to achieve that, we make a 
fuzzy search (finding approximate substring matches inside a given list of strings) and 
stop at the next occurrence, repeat this process while the user hit the search
button and if we reach the bottom of the list, the search restart automatically
at the top of the list.

### Pseudo code
```python
    function search(text, rec=True):
        """ Cyclic search inside summaries

        Keyword arguments:

        text -- expression searched
        rec -- keep track of recursive calls
        """
        sel = buffer.GetSelection()

        if rec:
            search_index = sel
        else:
            search_index = -1

        for i in range(search_index + 1, len(data) + 1):
            #  edge case : The last packet is selected
            if i >= len(data):
                found = False
                break

            found = text.lower() in data[i].lower()
            if found:
                search_index = i
                break

        if not found:
            if rec:
                search(text, rec=False)
        else:
            buffer.SetSelection(search_index)
```


### Complexity analysis

#### Time complexity
If we assume that the list `data` has n elements and the max size of elements
inside data is p ( p = max(data[i], 0 < i < n+1) ), in the worst case there are no matchs in the whole list, the complexity is then O(n).

#### Space complexity
The space corresponding is O(n)


# Evaluation of results
Typically, SMB 2 Protocol messages begin with a fixed-length SMB2 header that 
is 
described is the SMB specifications [@openspecs-officeMSSMB2ServerMessage]
The SMB2 header contains a Command field indicating the operation code that is 
requested by the
client or responded to by the server. An SMB 2 Protocol message is of variable 
length, depending on
the **Command field** in the SMB2 header and on whether the SMB 2 Protocol 
message is a client
request or a server response.

So basically, when you are trying to make diffs on SMB packets, there are two
possibilities:

- the command fields of the packets are different thus the packets aren't of the
same type
- the command fields are the same thus the packets are of the same type

## Diffs

### Different command fields

![Plain text output for different command fields](./img/plain_text_different_command.png)

\pagebreak

![PDML output for different command fields](./img/pdml_different_command.png)

\pagebreak

- **Interpretation**
Foobar
\pagebreak
### Same command fields

![Plain text output for same command fields](./img/plain_text_same_command.png)

\pagebreak

![PDML output for same command fields](./img/pdml_same_command.png)

\pagebreak

- **Interpretation**
Here it's even more visible, using the plain text output, it's very difficult
to separate a field from his value, that's what you can notice from the first 
output; the fields `Time for request`, `Preaut Hash` and `Current Time` are only
changing values but with the first version they are highlighted as one line
change while the second are hightlighted more precisely as values change.

\pagebreak

## GUI
One objective was to port to windows, 
the tests have been realized on Windows 10 Professional and Fedora Workstation
31 the views are following:

### Windows
![Windows view smbcmp](./img/smbcmp_windows.png)

\pagebreak

### Linux
![Linux view smbcmp](./img/smbcmp_linux.png)

\pagebreak


# Prospect
To improve the usefulness of our system, we can think of many things that can
be added or done differently:

## Add documentation for config file and usage

About :
- file format of the config file (possible options, possible values,...)
- the location it will be loaded from
- how to use -k, how to generate keys from kernel or samba

## Install documentation and desktop file in packaging

Modify packaging scripts for all OS to install the man pages and desktop file
to make it available on all major Linux distributions, actually it's only on 
Opensuse repository.

## Pass down arguments passed to windows launcher to the Python scripts

The windows port is not perfect, there are some features present on the Linux
version which aren't yet on windows.
Add arguments to smbcmp.exe (-h) and Drag and drop support to load capture files
more easily and intuitively.

## Implement ignored fields in smbcmp-gui

Add or remove ignored field from the diff view (right click)
Add a main menu item to list and manage ignored fields. Implement ignored 
fields filters :

- value greater than, equal to, less than
- value is one of x, y, z
- value contains x

We can think of creating a little programming language with its interpreter
which will be able to parse and validate rules.

# Implementation of CI/CD
podman obv https://cloudnweb.dev/2019/06/replacing-docker-with-podman-power-of-podman/


# Business spendings on AWS
On average, businesses waste about 35% of their cloud spend due to inefficiently using their cloud resources. This amounts to more than $10 billion in wasted cloud spend across just the top three public cloud providers.
DHI used about $38/Month with our researches this cost is half-reduced $19/month.

## Comparison between GCP and AWS
Type de machine	Elastic Compute Cloud	Compute Engine
Cœur partagé	t2.micro - t2.large	f1-micro
g1-small
Standard	m3.medium - m3.2xlarge
m4.large - m4.10xlarge	n1-standard-1 - n1-standard-64
Haute capacité de mémoire	r3.large - r3.8xlarge
x1.32xlarge	n1-highmem-2 - n1-highmem-64
Haute capacité de calcul	c3.large - c3.8xlarge
c4.large - c4.8xlarge	n1-highcpu-2 - n1-highcpu-64
GPU	g2.2xlarge
g2.8xlarge	Ajouter des GPU à la plupart des types de machines
Stockage SSD	i2.xlarge - i2.8xlarge	n1-standard-1 - n1-standard-64
n1-highmem-2 - n1-highmem-64 n1-highcpu-2 - n1-highcpu-64*
Stockage dense	d2.xlarge - d2.8xlarge	N/A

# Capacity planning steps
## First: Get a baseline


 .      | Current Capacity | Maximum capacity
---------|----------|---------
 CPU     | 2 - Quad core |	4 - Quad core
 RAM	|   128 GB | 512 GB
 Disk	| 2 Disks - 1 TB - RAID 1 |	6 Disks
 NIC	|2 GbE (Dual)|	6 GbE (Dual) - 10 GbE (Quad)

## Second: Set up performance monitoring
Monitoring a Linux distribution is pretty straightforward, with tools like
dtrace, htop, ... Interested in no particular metric, we decided to go from a
general purpose tool: sysstat @SysstatLinuxManual 

## Third: Analyze and plot data
The output provided by sysstat is a text-based one, so to analyze/interpret
data, a simple parser can be used, fortunately there's already a software built
for this purpose: kSar @sitnikovVlsiKsar2020.

## Fourth: Set performance thresholds
As a preliminary test, it's a common practice to set the CPU threshold at 
80% busy for all the servers.

## Fifth: Performance alerting
It's typically a notification (email, SMS, ...) sent by another service to alert
the maintainer of the server, in this case, it's handled by Amazon, via an e-mail.

# Sar (sysstat)
## cpu
Idle time is when the cpu is waiting for the disk.
## I/O 
### tps vs io/s
Transactions are single IO-commands (fetch block/write block) that are written to the RAW-disk (in your example dm-0). The linux-kernel tries to order those commands into a better sequence or tries to compress them into more efficient commands (like: get two blocks at once instead of get one block and get another block right after this one). These are the transactions that go out to the disk-controller (tps for sda).

### Kernel VFS 
Linux also has its own VFS, Linux VFS, whose purpose is not to be extensible, but to provide an interface to any imaginable file system. In addition, many operations can be supported by default functions, which greatly facilitates the development of the layer which is dependent on the file system.

Linux VFS adapts to the Linux6 native file model, in this case ext4. The basic abstractions of Linux VFS are therefore:

- Inspired by the structure of the ext4 system, such as superblock and inode objects;
- Modeled on the objects that allow Linux to manipulate the ext2 system, with the dentry and file objects.
This allows Linux to run its native file systems with minimal mediation

### Load
Load expresses how many processes are waiting in the queue to access the computer processor. This is calculated for a certain period of time, and the smaller the number the better.

We consider a load value under 10 to be acceptable.

# Conclusion
Capacity planning and performance monitoring work together to give you a complete picture of your hardware and software life cycle. It's important to take the time and make the effort to set up monitoring and alerting, and to analyze the data. Too often, busy system administrators set up elaborate solutions for monitoring and then ignore them. Find a way to strike a balance between being driven crazy by performance alerts and never seeing the one that results in extended downtime. Capacity planning also helps you save money by redeploying services from overutilized to underutilized systems.

# Appendix 

# Bibliography
