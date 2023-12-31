
# Personality/Motivational Dynamics in the Everyday Life of a College Student 




# Goal of the Model 

The goal of this project is to model the dynamics of motivation and behavior over time.  Researchers such as Berridge have argued that the degree to which we Want a particular Reward or Want to Avoid a particular Aversive state is a major driver of choice and behavior.   Wanting is argued to be the multiplicative result of the current relevant bodily or interoceptive state (e.g, Hunger) and the availability of relevant Motive Affordances (e.g, Food) in the situation.  

We also view this model as capturing the dynamics of the interaction between personality and situation over time, as individuals interact in different situations and with different people.  In a variety of publications, Lynn Miller and I have argued that personality traits can be viewed as goal-based structures, in which people's goals and motives are central to the representation of the trait.  That is, individual differences in traits and trait related behavior, are caused by chronic individual differences in goals and motives. We further argue that situations are also goal-based structures, in which a central part of the representation of a situation is the goals/motives whose satisfaction it affords and the behaviors that can be enacted to pursue those goals.  Thus, motivation (and therefore behavior) in different situations and across time are a function of the interaction between the individual's chronic Motives, the current affordances in the Environment, and the individual's current Interoceptive (or Need) state.  

In the model, the Motive layers represent the chronic motives of the individual (the central feature of a trait).  The activation of those nodes by Environmental Features and Interoceptive State contributes to the current or phasic state of those motives, which is the sum of the chronic activation of the Motives and the additional activation caused by the Environmental and Interoceptive inputs. The phasic or momentary activation of the motive nodes then drive behavior.


Following this idea, the current network models the following things:
- Environmental Affordances and Bodily or Interoceptive state have a joint, multiplicative role on the generation of Motive activation (Wanting) by sending activation to their corresponding motive nodes.
- The activation of Motive nodes is the sum of this input and the chronic activation. The chronic or baseline activation of a Motive can be modified by inputs from the MotiveBias Layer.  
- The activated Motive nodes then compete with each other, within the Approach and Avoid layers, and the "winning" nodes send activation to the behavior node. Strength of activation of the Motive nodes corresponds to what Berridge terms "Wanting". 
- Activated Behavior nodes then compete with each other and the winning behavior node then results in behavior.


## Modeling changes over situations and time

In addition to modeling the above with the basic network, the more advanced simulation represents a World that can change as a result of the individual's behavior, as well as exogenous factors, which then leads to future changes in behavior.  

- The selected Behavior node can change both:
	- Environmental Affordances and 
	- Interoceptive State
- Exogenous factors and changes over time (e.g., in Interoceptive State) can also change the Environment and Interoceptive state
- Resulting changes in Environmental Affordances and Interoceptive State then impact subsequent Wanting and Behavior

### The everyday life of a college student

The concrete domain we use is the everyday life of a college student.  We represent  major motives that  drive the behavior of a typical college student and some of the the major Situational/Environmental Affordances that a typical college student would encounter in everyday life.


**MotiveBias**. We can manipulate the chronic baseline activation of motives to capture some individual differences, by using the MotiveBias layer.  The nodes in this layer have one to one connections to the Motive Nodes.  The default values for the MotiveBias layer in the Training and Testing files are set to 0s, but this can be edited to increase the chronic, baseline activation of the corresponding Motives.  

**Dopamine.** In addition to the above, it is also possible to model the impact of tonic dopamine levels on Wanting or the strength of motivation.  Wanting is argued to be a function of Dopamine activity in the NAcc/Amygdala circuit.  Higher dopamine activity, both phasic and tonic, should lead to higher levels of Wanting.  In the current model, Dopamine levels are modeled by inputs from a VTA_DA system.  The inputs from the VTA_DA system are modeled by entries in the corresponding cells in the input data table.  The default Training and Testing DataTables have 0s entered in these cells. But the values can be edited.  


## Structure of Network 

The Network has the following major layers

- Environment - represents the Environmental Affordances 
- InteroState - represents the current Interoceptive State for corresponding Motives 
- Motives
	- Approach
	- Avoidance
- Behavior

There are also additional layers, which can be used to modify dopamine level and chronic motive activations.
- **VTA/DA layer*** - can be used to simulate impact of tonic/chronic DA on baseline activation of the Motives.  Has excitatory impact on Approach Motives and inhibitory impact on Avoid Motives
- **MotiveBias** - Can be used to modulate the chronic activation of a Motive

The individual nodes in the different layers are:

**Environment**

Frnd - Friend 

Lbry - Library

Food - Food

Mate - Mate

Bed - Bed

SocSit - Social Situation

Dngr - Danger

**Interoceptive State**

nAff -nAffiliation

nAch - nAchievement

Hngr - Hunger

Sex - Sex

Slp -Sleep

SAnx - Social Anxiety

Fear - Fear

**Approach Motives**

AFF - Affiliation

ACH - Achievement

HNGR - Hunger

SEX - Sex

SLP - Sleep

**Avoid Motives**

REJ - Avoid Social Rejection

HRM - Avoid Physical Harm

**Behavior**

Hngt - Hangout

Stdy - Study

Eat - Eat

AvSoc - Avoid Socializing

Sex - Have Sex

Sleep - Sleep

Lve - Leave

SHngt - Seek Hangout

SStdy - Seek Study

SEat - Seek Eat

SSlp - Seek Sleep

SSex - Seek Sex


## Structure of Program



### Training 


**NOTE:**  
In this version of the model, we are doing training that parallels separate Pavlovian and Instrumental training.  One run through trains the Pavlovian associations between Environment and Interoceptive State, and Motives.  A separate run through trains the Instrumental relationship between Motives and Behavior.  When switching between the two types of training, the type of the different layers is switched so that in Pavlovian training, Environment and Interoceptive Layers are Input Layers and Motive layers are Target Layers.  Then for Instrumental training, Motives are Input layers and Behavior is a Target layer.  Hidden layers remain set to Hidden and all other layers are set to Output, which does not participate in Learning.  

We programatically control the order in which the two kinds of learning are done by reading a DataTable that controls the order of learning and the number of epochs for each type of learning.  


Training programs  and relevant Training Data are used to teach the network two general things:
- It learns to calculate a roughly multiplicative relationship between the relevant environmental feature and the corresponding interoceptive state in predicting the relevant motive activation.  

- It learns a relationship between the strength of WANTING and the relevant  Behavior.

- The chronic or baseline activation of WANTING nodes can be modified by tonic input from DA/VTA neurons
- A MotiveBias layer can be used to manipulate individual differences in the baseline activation or importance of different Motives.


Training Data files are called:  

- instr.tsv -- Trains weights between Motives and Behavior only. 
- pvlv.tsv -- This file is structured to teach the relationship between Inputs and Motives.  

Two files are used to control the order of Pavlovian and Instrumental Training and the number of Epochs of each:

- InstrThenPvlv.tsv
- PvlvThenInstr.tsv

## Running program

Hit Init, then the button TrainPIT. This button will train the network in the two steps described above.  The Instr field has the table for the Instrumental training and the Pvlv field has the training tble for the Pavlovian training.  The Trn field has the file for information about the order of the two types of training and the number of epochs of each type of training.

Modifying the number of Epochs of training can be done by either editing the relevant file or by clicking on the 


## Modeling Motivation and Behavior Over Time

Currently, all the logic for how Behavior changes Environmental Affordances and Interoceptive State is in the XXXXXX function

New events are presented to the network from the input_data Table, currently named World_Changes.tsv. A datatable named World.tsv represents the current State of the World and is update with each event.  

Once the network settles, the Program identifies the mostly highly activated behavior from the Behavior layer and based on that behavior updates both the Environment, if relevant, and the corresponding Interoceptive state that are represented in World.tsv.  

These modified values for the Environment and the Interoceptive State are then fed back into the network for the next event.

Environment and Interoceptive State are changed in three ways.
- Some Interoceptive states simply change over time.  For example, with the passage of time one gets hungrier or has a greater need for social affiliation.  The program in  changes these states with each time step as a function of an increment that can be modified by the user.  Different increments can be set for different Interoceptive States.
-  Environment and Interoceptive State can be changed by an enacted behavior.  Eating changes both Environment and Interoceptive State.  Hanging out with friends reduces the need for Affiliation. This is handled by the program using a parameter that is set by the user.  Different parameters for each one.  
- Environments sometime changes because of State changes:  Friend enters or leaves the situation. Weather changes. Fire alarm goes off.  Intruder shows up with gun, etc. 

Oftentimes there can be a delay in the impact of an action on Interoceptive state. For example, it takes a while for eating to change level of Hunger.  This delay can be set indepependently for each Interoceptive State.  

Currently, a time step in the program is equivalent to the presentation of a single state of the world to the network and the generation of a behavior.

This is the logic for presenting a sequence of events to the network.  

- DataTable assigned to input_data (World) is the one directly presented to the network. It represents the current State of the World, both internal and external. The initial row represents the starting state of the world. The results of this event are then used to calculate Changes in the World. These Changes caused by behavior are added to exogenous Changes represented in the WorldChanges table and then all changes are copied to the next row of the input_data table_. 

Results from the current selected behavior and information from the input_data_load table about changes of State are integrated and then copied to the next row of the input_data table World_State, which represents the current State of The World.  This is the next event seen by the network.

- DataTable assigned to WorldChanges represents **Exogenous CHANGES** in the State of the World over time. The first row in this table is blank.  Rest of the table represents changes from this initial state, with each subsequent row representing a following event.

- WorldChanges DataTable represents when a feature appears in the environment and when it disappears from the environment.  The feature is NOT repeated in the DataTable as long as it exists. This table ONLY represents CHANGE of STATE. If a feature enters, it is represented by a positive value between 0 and 1. If a feature leaves, such as a friend leaving the environment, then this is explicitly indicated by a -1, which reduces the presence of this feature down to 0 (Program limits values to range of 0 to 1).  Thus, this table represents <b> CHANGES </b> in the state of the world. Represents when a feature appears and when it disappears. Once a feature appears and has been flagged by an entry in this table, there is no further entry until the feature disappears which is indicated by a -1.


- This is the next event seen by the network.


The program uses the following variables.

### Variable descriptions 

- _decr -- amount of change in Interostate (or Environment) per relevant behavior (e.g., consummation or satiation).  Could be either positive or negative. 

- _incr  -- amount of change in either Environment or Interostate as a result of passage of time. Typically positive, although some events could have negative effect. 



_incr indicates increases in interceptive states that are not  a direct result of behavior.  Instead they occur simply due to the passage of time. They increase regardless of whether the delay condition is met. So for example, studying would decrease need for achievement after delay, but other needs should steadily increase. So when you start to study, needs like hunger, sleep, need for affiliation would steadily increase, and they should increase before delay is met. The only needs that won't steadily increase right now are avoid harm and social anxiety. 
