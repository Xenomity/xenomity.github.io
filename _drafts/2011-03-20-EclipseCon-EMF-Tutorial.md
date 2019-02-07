---
title: "EclipseCon EMF Tutorial"
tags: [eclipse, emf, osgi]
date: 2011-03-20T15:56:00+09:00
---

출처 : [http://eclipsesource.com/blogs/](http://eclipsesource.com/blogs/)  
원제 : What every Eclipse developer should know about EMF  
  
EclipseSource에 EclipseCon에서 발표되었던 EMF 튜토리얼이 파트별로 올라오고 있다...!!

* * *

## Table of Contents

1 Introduction

2 Example Model

3 Modeling

3.1 Code Generation

3.2 Model refinement

3.3 Why is this better than writing POJOs?

4 EMF API

5 Import Sample Solution

6 AdapterFactories

7 EMF Data-Management

8 EMF UI

9 Additional EMF-based Technologies

## 1 Introduction

For the question “What is EMF” we borrow the description from the EMF website:

“The EMF project is a **modeling framework** and **code generation** facility for building tools and other applications based on a **structured data model**. From a model specification described in XMI, EMF provides tools and runtime support to produce a set of **Java classes** for the model, along with a set of **adapter classes** that enable **viewing** and **command-based editing** of the model, and a **basic editor**.”

It is noteworthy that EMF has also been a stable standard for many modeling technologies.

We suggest using EMF for any structured data model you have to create in Eclipse, especially if it gets stored, displayed and modified in UIs. The basic EMF workflow is very pragmatic; a model is created defined in the Ecore format. Ecore is basically a sub-set of UML Class diagrams. From an Ecore model, you can generate Java code.

[![ecoreWorkflow1 300x167 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/ecoreWorkflow1-300x167.png "ecoreWorkflow")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/ecoreWorkflow1.png)

Later in this tutorial we will have two runing instances of Eclipse. In the first instance, the “IDE”, we will define the model and generate code from it. The second instance, the “Runtime”, will be started from the IDE and contain instances of the generated model.

[![idevsruntime 300x123 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/idevsruntime-300x123.png "idevsruntime")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/idevsruntime.png)

## 2 Example Model

In this tutorial we will create an example model for managing a bowling league and tournaments. A Leagues contains an arbitrary number of Players. A Tournament consists of an arbitrary number of Matchups. A Matchups always contains of two Games. A Game is a list of frames (the score) and is assigned to a certain Player. Finally a Tournament has a Enumeration determining the type of the Tournament.

[![bowling 300x150 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/bowling-300x150.png "bowling")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/bowling.png)

In the next step, we will show how to create this model and generate code form it.

## 3 Modeling

We will create our example model in EMF to generate the entity classes for our application. The first step is to create an empty modeling project in your workspace:

[![1newEmptyEMFProject 300x285 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/1newEmptyEMFProject-300x285.png "1newEmptyEMFProject")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/1newEmptyEMFProject.png)  
  
[![2nameProject1 300x172 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/2nameProject1-300x172.png "2nameProject")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/2nameProject1.png)

The essential part of the modeling project is the model itself, defined in the format “Ecore”. Please create a new “ecore” file in the model folder in your new modeling project. We will name the file “bowling.ecore”:

[![3newecore 300x285 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/3newecore-300x285.png "3newecore")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/3newecore.png)   
  
[![4namemodel 300x263 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/4namemodel-300x263.png "4namemodel")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/4namemodel.png)

Click finish to create the model. It will be opened in the default Ecore editor. This editor allows defining Ecore models in a tree-based view. There are several additional possibilities to define Ecore models, including graphical modeling, textual modeling, Java annotations and import from UML tools. We will stick to the default editor in this tutorial and shortly demonstrate the graphical editor for Ecore.

In the tree of the Ecore editor you can create and delete model elements as well as modify the structure of your model via drag and drop. Properties of model elements can be modified in a second view, which opens up on double-click or right-click =\> “Open Properties View”.

First, give the root package of your new model a name and an URI. This is used to identify the model later on. Name the package “bowling”, set the Ns Prefix to “org.eclipse.example.bowling” and the Ns URI to “http://org/eclipse/example/bowling.ecore”:

[![5namerootpackage 300x156 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/5namerootpackage-300x156.png "5namerootpackage")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/5namerootpackage.png)

Now, we can define our model elements as children of the root package. Please right click the root package and create a new EClass and name it Player.

[![createPlayer 300x191 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/createPlayer-300x191.png "createPlayer")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/createPlayer.png)

From the context menu of an EClass, you can add EAttributes and EReferences to it. Create an EAttribute and open the Property view for it. The properties of an EAttribute define its name, its data-type and other properties, which we will cover later in this tutorial. Add an EAttribute to the Player, name it “name” and assign the EType “EString” (java.lang.string). Repeat this step and add a second EAttribute named “dateOfBirth” of type “EDate”.

[![nameattribute 300x239 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/nameattribute-300x239.png "nameattribute")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/nameattribute.png)

EMF models usually build up a structured hierarchy, meaning model element instances, e.g. the Player “Jonas” are contained in a specific container object. This provides a tree-structure, which is useful for navigability and allows for serialization. This tree structure is often referred to as containment tree. In our model, Players are contained in a League. Please note that this also implies that every Player is referenced by exactly one League, thus cannot be part of more than one League. Therefore EMF will automatically take care that a player cannot have more than one league. If you add a player to another league, its reference to the original league vanishes.

[![leagueplayer What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/leagueplayer.png "leagueplayer")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/leagueplayer.png)

Create a second EClass and name it “League”. To identify the League, also create a EString attribute called name. The next step is to create a EReference between League and Player by right-clicking on the League model element. Name the references “players”. Set the EType of the reference to “Player”. As a League can contain an arbitrary number of Players, set the upper bound to “-1”, equivalent to “many”. Finally set the property Containment to “true” determining that the EReference is a containment reference.

[![referenceplayers 204x300 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/referenceplayers-204x300.png "referenceplayers")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/referenceplayers.png)  
  
After this first model iteration, we can already generate code from it. EMF provides an example editor to create and modify model instances. This allows us, to initially test the created model, before we further refine and add more EAttributes and EReferences in a second iteration to complete the example model.

## 3.1 Code Generation

In this step, we will generate the entities from the Ecore file we have created. Please note that you can regenerate the entities again, if you need to change you model. EMF can deal with simple changes like adding model elements or EAttributes. If you have complex changes, like moving an attribute to another class, you would have to migrate existing instances of the model. This is supported by the EDAPT framework.

To generate entities, we have to create a generator model file first. This file allows configuring properties for the generation, which are not part of the model itself, for example the plugin and subfolder, the source code is generated to. Please create a new generator file in the model folder and select our previously created Ecore as a source model.

[![creategenmodel 300x285 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/creategenmodel-300x285.png "creategenmodel")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/creategenmodel.png)

[![nameGenModel 283x300 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/nameGenModel-283x300.png "nameGenModel")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/nameGenModel.png)  
  
[![selectecore 300x255 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/selectecore-300x255.png "selectecore")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/selectecore.png)  
  
[![choosemodel 300x161 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/choosemodel-300x161.png "choosemodel")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/choosemodel.png)  
  
In the root node of the generator model, you can set the properties for the code generation. In the tree of the generator model, we can set properties for every generated entity. For the first code generation, we just use the default settings. Based on the generator model, we can now generate the source code. EMF allows generating four different plugins for a defined model:

- **Model** : The model contains all entities, packages and factories to create instances of the model.
- **Edit** : The edit plugin contains providers to display a model in an UI. For example the providers offer a label for every model element, which can be used to display an entity showing an icon and a name.
- **Editor** : The editor plugin is a generated example editor to create and modify instances of a model.
- **Test** : The test plugin contains templates to write tests for a model.

To generate the plugins, right click on the root node of the generator model and select the plugin. For our tutorial, please select “generate all”.

Before we look at the generated code, let’s first start the application and create an entity of our model. Please right click on the model plugin and select “Debug as =\> Eclipse Application”.

Please create a new empty project and a bowling model. This file will contain a serialized version of our model instance.

[![createproject1 300x248 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/createproject1-300x248.png "createproject")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/createproject1.png)  
  
[![createbowlinginstance 300x285 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/createbowlinginstance-300x285.png "createbowlinginstance")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/createbowlinginstance.png)

[![namemodelinstance 300x271 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/namemodelinstance-300x271.png "namemodelinstance")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/namemodelinstance.png)

Select League to be the model object. This determines the root object of the model instance we are about to create.

[![seletcroot 300x227 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/seletcroot-300x227.png "seletcroot")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/seletcroot.png)

The generated editor for model instances works similar to the Ecore editor. Model element instances can be created via a right-click, EAttributes can be modified in the properties view. Please name the League and create two Players. On save, all created instanced are serialized in the XMI file “league.bowling”.

[![ecore editor 300x148 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/ecore-editor-300x148.png "ecore editor")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/ecore-editor.png)

## 3.2 Model refinement

Let`s switch back to our IDE Eclipse environment, complete the model and regenerate the source code. In this second model iteration we will add different type of EReferences, as well as EEnums and Multi-EAttributes. First please add the following classes to the bowling model:

- Tournament
- Matchup
- Game

These classes model the results of bowling tournaments build up a second tree in our model. Therefore we add containment EReferences from Tournament to Matchup and from Matchup to Game. Following the bowling rules, a Matchup consists of two Games (each from one Player). We model this by setting the upper bound and lower bound of the EReference “games” of the EClass Matchup to “2”.

[![multiplicitytwo 300x189 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/multiplicitytwo-300x189.png "multiplicitytwo")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/multiplicitytwo.png)

Furthermore we defined that the EReference between Matchup and Game is bi-directional. That means, the reference can be navigated from both ends. Therefore we have to create a second EReference from Game to Matchup and bind both EReferences. EMF will take care of the bidirectional synchronization. In other words adding a matchup to a game will automatically add the game to the matchup.

Please add a EReference to Game called “matchup” with the EType “Game”. By setting the EOpposite to the EReference “games”, both EReferences are coupled bi-directionally. Note, that the property “Container” will automatically be set to true.

[![bidirectionalreference 300x198 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/bidirectionalreference-300x198.png "bidirectionalreference")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/bidirectionalreference.png)

The next step is to add a cross-EReference. In contrast to containment EReferences, cross-referenced model elements do not contain each other. In our model we add a cross EReference from Game to Player named “player”. Let both, container and containment properties “false”. An arbitrary number of games can be assigned to a Player now; the Player is still contained in a League.

[![gameplayerref 300x194 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/gameplayerref-300x194.png "gameplayerref")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/gameplayerref.png)

As a final mandatory step, we will create an EEnumeration for the type of a tournament. A tournament can be of type “Pro” and “Amateur” in our model. Please create a EEnum by right-clicking on the root bowling model package, just like creating a class. Add two EEnum Literals to this EEnum.

[![enum 300x136 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/enum-300x136.png "enum")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/enum.png)

Please add an EAttribute to the EClass Tournament, name it “type” and set the EType to “TournamentType”.

The extended example model contains more EAttributes and EReferences to be added including all basic types and some special cases as the Multi-Integer EAttribute in Game. Depending on your time, please model the following features:

Player

- height: EDouble
- isProfessional: EBoolean

Game

- frames: EInt, UpperBound = 10

After applying complex changes to the model it is always a good idea to validate it, done with a right-click on the model root in the Ecore editor. Let`s do something wrong in the model and set the lower bound of the EAttribute “games” (in Matchup) to 3. As the upper bound is 2, this model doesn`t make too much sense. This will be detected by the model validation, something which is impossible in plain Java Code.

[![validation 300x148 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/validation-300x148.png "validation")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/validation.png)

After this model refinement, we will re-generate the code to reflect our changes. Start the runtime Application again and create a second model “tournament”. Add a Matchup and two Games. To add assign the Games to Players you will have to load the “league” model created before. Select “Load Ressource” from the menu “Bowling Editor” and select the first model file. Now, link the Games to the Players in the properties view.

## 3.3 Why is this better than writing POJOs?

You might ask, why to use EMF instead of creating the model just writing plain POJOs. Without considering benefits like the generated editor for rapid testing and all additional frameworks available for EMF, let`s look at two very simple and exemplary benefits.

Before we even look at the generated code (we will do that in a minute), let`s consider the amount of code we have just produced. The Eclipse metrics plugin tells us, we have generated over 1000 LOC, while only 150 is part of utility classes. Even very simple code is considered to be worth 1$ per LOC. So we have just earned 1000$ just by clicking some buttons?

[![locs 300x86 What every Eclipse Developer should know about EMF Part 1](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/locs-300x86.png "locs")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/locs.png)

In the next part, which I will post this week, we will explore the EMF API of the code we have just generated. You can follow this blog to get notified about new posts.

## 4 EMF API

In this part of the tutorial, we will explore EMF’s API, including the generated code, as well as EMF’s utility classes. Let’s have a look at the generated code first. In the model plugin from our tutorial org.eclipse.example.bowling you will find interfaces and implementations for all entities of the model. A look at the outline of an entity’s interface reveals that it contains getters and setters for the attributes we have defined in the model, as well as access to the references. All entities of the generated EMF model are sub-classes of EObject. EObject contains basic functionality – for example, a change notification mechanism.   
  
[![outline What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/outline.png "outline")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/outline.png)   
  
The model plugin contains factories to create model element entities. Please note that the constructor of EObjects is usually not public. Please also note, that factories are used by many frameworks for their functionality, e.g. deserialization. Changing these methods successfully requires some careful planning. Let’s use the factories to programmatically create some entities and use their API to modify them. We will use the pre-generated test plugin to run this example code. In this very simple example, we use the BowlingFactory to create a Matchup and a Game, adding a reference to the Matchup and checking the bi-directional update on the Game.   
  
[![tescase01 What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/tescase01.png "tescase0")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/tescase01.png)  
  
The super class EObjects offers many methods to access an entity in a more generic way. As an example, we will test the containment between Matchup and Game by accessing the EContainer instead of the getMatchup() method.  
   
[![testcaseone What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testcaseone.png "testcaseone")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testcaseone.png)  
   
EObjects offer reflective access to their attributes using the methods eSet() and eGet(). This can be useful in modifying an entity in a generic way.   
  
[![testcase3 What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testcase3.png "testcase3")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testcase3.png)  
   
Information about the available EAttributes and EReferences, as well as all additional concepts we have modeled before, can be accessed either through the EClass or through the EPackage. The following test checks whether the only EReference of League is a multiplicity greater than one.  
 [![testcase4 What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testcase4.png "testcase4")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testcase4.png)  
   
EMF also supports the validation of model instances. As an example, we can validate the constraint of the model that a matchup must always consist of two games:  
 [![testvalidation What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testvalidation.png "testvalidation")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/testvalidation.png)  
   
Finally EMF provides a lot of utility classes. A very important one is EcoreUtil. It is worthwhile browsing through the available methods of EcoreUtil. As an example, we will use the copy method to create a copy of an EObject.

[![tesecoreutil What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/tesecoreutil.png "tesecoreutil")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/tesecoreutil.png)

## 5 Import Sample Solution

Before we continue with the tutorial, please import the sample solution from the links above. Please switch to an empty workspace (File =\> Switch Workspace), select “Import” =\> ”General” =\> ”Existing Projects into Workspace”. Please select “exampleSolution.zip” and   
import all projects.  
  
[![select projects to import 300x282 What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/select-projects-to-import-300x282.png "select projects to import")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/select-projects-to-import.png)

## 6 AdapterFactories

For the next steps of the tutorial it is important to understand the concept of AdapterFactories. We will give a quick introduction and you can see [<font color="#084081">this documentation</font>](http://publib.boulder.ibm.com/infocenter/wasinfo/v6r0/index.jsp?topic=/org.eclipse.emf.doc/references/overview/EMF.Edit.html) for more information. The basic idea of AdapterFactories is to provide you with an interface IX you need for an arbitrary purpose, e.g. a LabelProvider needed in the UI. EMF generates a lot of these classes for you. To retrieve the right class you can use an AdapterFactoryX, which implements the interface you need. The AdapterFactoryX will retrieve the generated classes using an AdapterFactory.  
  
 [![adapterfactories 300x158 What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/adapterfactories-300x158.png "adapterfactories")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/adapterfactories.png)

## 7 EMF Data-Management

In the previous sections we have shown how to generate a structured data model with EMF. In a typical application, these data models have to be stored and maybe even versioned or distributed. There are a couple of frameworks that support different use cases. By default EMF provides the ability to serialize EObjects to XMI files. In the following example, we will load EObjects from a file and save them afterwards. EMF also offers commands to make modifications on the model. Commands can be easily undone. In the example we will load a XMI file containing a Tournament. The user can add new Matchups to that Tournament and undo these changes. At the end, the user can save the changes back to the file. For the tutorial, we have prepared an example dialog in the plugin org.eclipse.example.bowling.tutorial, which should be imported from the example solution. You can open this dialog by right-clicking a file and selecting “Tutorial”=\>”Open Example Dialog”. After implementing the following two parts of the tutorial, it will look like this:   
  
[![example view 300x134 What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/example-view-300x134.png "example view")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/example-view.png)   
  
Everything that is not relevant for this tutorial is implemented in an abstract base class called AbstractExampleDialog. In the subclass ExampleDialog, there are empty method stubs to be implemented in this tutorial. The dialog can be shown in the runtime Eclipse instance by right-clicking on a file and selecting “Open Tournament Example view”. (Please note that for the tutorial we have focused on simplicity over perfect design.)

Please open the class ExampleDialog. We will implement the loadContent method, which is triggered by opening the example view. The purpose of this method is to get a Tournament from the file which is then displayed in the example view. To keep it simple, we assume that the file contains a Tournament and this Tournament is the first element in the file. You can easily create a file like this with the generated example editor. First we create an editing domain. An editing domain manages a set of interrelated models and the commands that are run to modify them. For example, it contains the stack of all former commands. An editing domain can create a resource, which is a container for storing EObjects. Resources can be saved and loaded and contents can be added to them. In the example we get the first EObject in the resource, assume it is a tournament and make it a member of our superclass.  
 [![loadContent What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/loadContent.png "loadContent")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/loadContent.png)  
   
After loading the content, we will implement a save. This will be triggered by pressing OK in the dialog and will serialize the model and apply all changes to the file.   
  
[![save What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/save.png "save")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/save.png)   
  
Now we want to implement the addition of a matchup to a tournament. We will use a command for this. First we create a matchup using the appropriate factory. The factory, by convention has the name of the base package of the model. Then we create a command which adds the newly created matchup to the existing tournament that we retrieve from our superclass. Finally, we run the command on the command stack of the editing domain.  
 [![addmatchup What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/addmatchup.png "addmatchup")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/addmatchup.png)  
   
At this point, the changes will not be reflected in the UI of the dialog, but we will implement the necessary code in the next step of the tutorial. To implement undo, we just need to call undo on the command stack of the editing domain, this will undo the last command.  
   
[![undo What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/undo.png "undo")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/undo.png)  
   
Now, start the bowling application and create a XMI file with the example editor. It should contain a Tournament and several Matchups and Games. Please right-click the file and select “Tutorial” =\> ”Open Example Tournament View”. In this view you can add new Tournaments, undo this operation and save by clicking on “OK”. You can validate the result by opening the file in the Ecore editor. Please note again, that the UI of the View will not be updated yet, but we will initialize the UI also in the next step of the tutorial.

### Additional Information

Ignite Talks on Persistency Frameworks: [<font color="#084081">EMFStore</font>](http://www.youtube.com/watch?v=YrqEUjAo82U&feature=player_embedded), CDO (coming soon)

## 8 EMF UI

In this step we will fill the example view with two basic UI elements. First, we will bind the Label on top showing the number of Matchups in the opened Tournament to the model. We will use the notification mechanism to update the Label whenever the number of Matchups changes. Second we will fill the TreeViewer with a list of the Matchups also displaying their Games as children. For updating the Label we will register a listener on the Tournament EObject, which is opened in the view. This listener will always be notified by the EMF runtime if there is a change in the Tournament EObject.  
   
[![initialize listener What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/initialize-listener.png "initialize listener")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/initialize-listener.png)  
   
In the second step we implement the listener itself. In the notify method, we check first whether the change was on the EReference to Matchups and therefore influences the number of Matchups. If this is the case, we update the Label (implemented in the AbstractExampleView).  
   
[![updateNumberOfMatchups What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/updateNumberOfMatchups.png "updateNumberOfMatchups")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/updateNumberOfMatchups.png)  
   
Please note that this is how you manually implement listeners. For creating UIs with bi-directional updates between UI elements and data-models, we recommend using data-binding that’s also available for EMF. In Eclipse databinding, you can bind a certain UI element to a certain EAttribute or EReference and it will take care of bi-directional updates.

Finally, we will initialize the TreeViewer to display the Matchups of the current Tournament and their Games as children. A TreeViewer needs three things to be initialized. The ContentProvider is responsible for the structure of the Tree by providing the method getChildren(). The LabelProvider is called to get an Icon and the displayed text for one node. The input of a TreeViewer is the invisible root element of the Tree. The elements displayed in the root of the tree are the children of that element. In our case, the input is the Tournament.

[![provider 300x103 What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/provider-300x103.png "provider")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/provider.png)

ContentProvider and especially LabelProvider usually depend on a certain EClass. EMF generates providers for several purposes including Content- and LabelProvider. We will use the AdapterFactory concept explained previously to retrieve the right provider for every element. Finally we set the Input to the Tournament which is currently open.

[![initializeTreeViewer What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/initializeTreeViewer.png "initializeTreeViewer")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/initializeTreeViewer.png)

Now you’ll need to re-start the bowling example application to test the implemented UI features. To modify the appearance of a Label you can simply modify the ItemProvider of the respective class. Let’s modify the LabelProvider for Matchup. To modify the appearance of EObjects in the TreeViewer you can adapt the generated ItemProvider GameItemProvider. In the example, we will show the number of Games contained in a Matchup Game. Mark the method as “generated NOT” to prevent it from being overridden during the next generation.

[![adaptLabelprovider What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/adaptLabelprovider.png "adaptLabelprovider")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/adaptLabelprovider.png)

In the running application, the new LabelProvider is displayed in the Example view as well as in the Ecore Editor:

[![labelproviderexample What every Eclipse developer should know about EMF – Part 2](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/labelproviderexample.png "labelproviderexample")](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/labelproviderexample.png)

As a last step, you should remove all listeners when closing the view. Please note that LabelProvider and ContentProvider are registered listeners on the model, so you should delete them.

### Additional UI Frameworks

There are several frameworks for displaying data from an EMF model instance. In this tutorial, the following were introduced in Ignite talks:

- [<font color="#084081">Extended Editing Framework</font>](http://www.slideshare.net/mchv/eef-sexy-properties-wizards-and-views-eclipsecon-11)
- [<font color="#084081">EMF Client Platform</font>](http://www.youtube.com/watch?v=0cwRzQU3GJY&feature=player_embedded)
- [<font color="#084081">Graphical Modeling Framework</font>](http://www.slideshare.net/mchv/gmf-create-your-graphical-dsl-eclipsecon-11)

## 9 Additional EMF-based Technologies

In this last part of the tutorial we want to give you a short overview of additional EMF-based technologies for different purposes:

- EMF Compare: Ignite Talk (coming soon)
- [<font color="#084081">EMF Query: Ignite Talk</font>](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/query2.pdf)
- [<font color="#084081">EDAPT: Ignite Talk</font>](http://eclipsesource.com/blogs/wp-content/uploads/2011/03/Edapt_EclipseCon_2011.pdf)
- [<font color="#084081">XText: Website</font>](http://www.eclipse.org/Xtext/)

I hope you found the tutorial helpful. In case you have any feedback or you are missing a certain framework or part, please feel free to contact [<font color="#084081">me </font>](mailto:jhelming@eclipsesource.com)or leave a comment.

