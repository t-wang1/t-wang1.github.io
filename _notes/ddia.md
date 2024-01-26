---
layout: archive
title: "Designing Data-Intensive Applications"
permalink: /notes/ddia
date: 2024-1-25
---

These notes are _Designing Data-Intensive Applications_ by Martin Kleppmann

### Resources/Links
* https://github.com/ms2ag16/Books/blob/master/Designing%20Data-Intensive%20Applications%20-%20Martin%20Kleppmann.pdf

### Reliable, Scalable, and Maintainable Applications

There are 3 main aspects of software systems that developers are concerned with: reliability, scalability, and maintainability. 

Having a reliable system stipulates that a program will continue to run correctly, even when it runs into faults. Kleppmann makes a distinction between faults and failures. Faults occur when a component of the system fails. Failures, however, occur when the entire system stops providing the required service. Developers are more concerned with preventing faults from becoming faults as opposed to preventing all faults. This should intuitively make sense because there would be a huge cost associated with preventing all faults, especially ones that don't have a high probability of occuring. 

Scalability deals with whether a system can be reliable with increased load. Vertical and horizontal scaling are both ways of coping with increased load. In vertical scaling, we move to a more powerful machine (ex. increasing CPU, RAM). In horizontal scaling, the load is distributed among multiple, smaller machines. 

There are three design principles associated with maintainability - operability, simplicity, and evolvability. Good operability makes routine tasks easy, allowing the operations team to focus their efforts on high-value activities. Abstraction removes _accidental complexity_. Evolvability makes it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases. 