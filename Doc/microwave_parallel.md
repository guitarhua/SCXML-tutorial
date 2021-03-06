# [Microwave owen (using parallel) example](https://www.w3.org/TR/scxml/#N11619)
The example below shows the implementation of a simple microwave oven using **\<parallel\>** and the SCXML **In()** predicate.

![microwave_owen_parallel](https://user-images.githubusercontent.com/18611095/46284787-d2a0b280-c581-11e8-8828-914b3cf87d34.png)

```
<?xml version="1.0"?>
<scxml xmlns="http://www.w3.org/2005/07/scxml"
       version="1.0"
       datamodel="ecmascript"
       initial="oven"> 

  <!-- trivial 5 second microwave oven example -->
  <!-- using parallel and In() predicate -->
  <datamodel>
    <data id="cook_time" expr="5"/>
    <data id="door_closed" expr="true"/>
    <data id="timer" expr="0"/>
  </datamodel>

  <parallel id="oven">

    <!-- this region tracks the microwave state and timer -->
    <state id="engine">
    <initial>
      <transition target="off"/>
      </initial>

      <state id="off">
        <!-- off state -->
        <transition event="turn.on" target="on"/>
      </state>
      
      <state id="on">
      <initial>
        <transition target="idle"/>
        </initial>
        
         <!-- on/pause state -->
        
        <transition event="turn.off" target="off"/>
        <transition cond="timer &gt;= cook_time" target="off"/>
        
        <state id="idle">
          <transition cond="In('closed')" target="cooking"/>
        </state>
        
        <state id="cooking">
          <transition cond="In('open')" target="idle"/>

          <!-- a 'time' event is seen once a second -->
          <transition event="time">
            <assign location="timer" expr="timer + 1"/>
          </transition>
        </state>
      </state>
    </state>

    <!-- this region tracks the microwave door state -->
    <state id="door">
      <initial>
        <transition target="closed"/>
      </initial>
      <state id="closed">
        <transition event="door.open" target="open"/>
      </state>
      <state id="open">
        <transition event="door.close" target="closed"/>
       </state>
    </state>

  </parallel>

</scxml>
```
