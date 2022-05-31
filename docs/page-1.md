---
description: PBFT State Machine
---

# Page 1



### PBFT State Machine:

```
     reset clock to 0                                   start at clock 2 lambda              clock (2 lambda, 4 lambda)
|------------------------|  wait until to 2 lambda    |-------------------------|          |---------------------------|
|     proposal state     | ----------------------->   |       filter state      | -------> |      certify state        |
|------------------------|                            |-------------------------|          |---------------------------|
   ^               ^                                                                                       | 
   |               |                                                                                       | wait until to 4 lambda
   |               |                                                                                       |
   |               |                                                                                       V
   |               |                                                                start at 4 lambda, clock (4 lambda, infinity)
   |               |                  enough next votes                             |-----------------------------------------|
   |               |----------------------------------------------------------------|                finish state             |
   |                                                                                |-----------------------------------------|
   |                                                                                           |                    ^
   |                                                                                           |                    |    
   |                                                                                           |                    | no enough next votes
   |                                                                                           |                    | AND wait 2 lambda time
   |                                                                                           V                    |
   |                                                                                         clock (4 lambda, infinity)
   |                       enough next votes                                        |-----------------------------------------|
   |--------------------------------------------------------------------------------|          finish polling state           |
                                                                                    |-----------------------------------------|
```
