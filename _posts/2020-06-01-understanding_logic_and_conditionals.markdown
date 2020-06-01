---
layout: post
title:      "Understanding Logic and Conditionals"
date:       2020-06-01 03:09:38 -0400
permalink:  understanding_logic_and_conditionals
---


One of the best way to understand logic expressions and conditionals is to read it out loud. I found myself that when I would just read them in my mind, I would have more difficulty understanding them. By just reading these statements out loud, it would be more easier for me to hear if it actually made sense. Some thing that I keep in mind is that logic expressions and conditionals are not about memorization but about truly understanding the concepts.

For example, understanding that AND (&&) means both sides of a statement have to be true for the entire statement to be true makes sense if you imagine you have an apple and an orange inside a basket and say out loud “there is an apple here and there is an orange here, so it true I have both.” Then it becomes clear that if one or both is/are missing the statement is false.
Using this example and talking to one self about it makes it easier to understand OR ( | | ) and ! (not) statements as well.  

Unlike logical expressions, there is some initial memorization when it comes to conditionals. This mainly involves remembering the syntax for IF statements. If we want to have control over what we want our code to do, we must also tell it when to do it. In this way, it makes sense to have a condition stated after the IF keyword, and then followed by the code we want to execute. Moreover, when using conditionals, it is important to keep in mind that we need to tell our code what to do when a condition is not met by using the ELSE and ELSIF keywords, remembering that when using ELSIF, it needs to be before ELSE and can be used as many times as we want as shown below. 
		
		if people < cats          <-initial condition
		   puts “There are more cats than people.”	   <-code to be executed if condition is true
		elsif people == cats          <-when initial condition is false, this condition is checked
		   puts “There is an equal number of people and cats.”     <-code to be executed if condition is true
		else
		   puts “There are more people than cats.”		<-code to be executed when all other conditions are false
		end

NOTE: Notice that after else there is no condition stated. That is because there is no other situation or possible scenario that can occur in this example.

Just like logical expressions, I believe that conditionals can be more easily understood when taking the time to read each statement out loud and talking yourself through it. Coming up with analogies or making up examples as mentioned above is also a great way to understand and practice what you are learning. 
