# CSharpLiveProject
This repository is dedicated solely to highlighting the contributions I made to the live project I did with my Code Bootcamp. In this live project me and my fellow students worked on a CMS (Content Managment System) for local theather in Portland. The CMS itself was made with ASP .Net MVC and Entity Framework. It's also meant to help manage login capability for subscribers, and log past performances and performers. The next section of this readme document will talk about my specific contributions and the skills I sharpened along the way.
___

  My first story I automated the process of counting the number of contributing devs to theater CMS with pure JS. I did this by counting the p tag, because it was only being used for names. So I wrote a small piece of script that stores the amount of p tags on the document in a varible and places the value next to the H2 tag.  

This is the script itself

    function countName() {
        var count = document.getElementsByTagName("p").length; 
        document.getElementById("Display").innerHTML = count;
      }
      countName();
  
  ___
## First Story in my assigned area
  In this story I was to create the entity model for prop rental history. 
    
  This is the code for the model 
 
    public class RentalHistory
    {
        public int Id { get; set; }
        public bool RentalDamaged  { get; set; }
        public string DamagesIncurred { get; set; }
        public string Rental { get; set; }
    }
    
  From this model and the pre-existing DB context I scaffolded the corresponding CRUD pages.  
  ___
  ##Second Story | Style create and edit pages
    Now it was time to add some styling to the create and edit pages based of a provided image. 
  I turned create and edit pages into a replica of the images with bootstrap and this is how it turned out.
    
   ![createeditStyle](https://user-images.githubusercontent.com/77030485/121847673-8f91b980-ccae-11eb-8c77-c849f1755db0.PNG)
  
  Implementing this lovely design was not the end of this story. Now I had to find to find a way to update the header text where it says notes to change to "damages" when the checkbox was click. 
  
  ### This was my solution
  
    var Counter = 0;
    function checkLink() {
      Counter += 1;
      if (Counter % 2 == 0) {
        document.getElementById("Switch").innerHTML = "Notes"
      }
      else {
        document.getElementById("Switch").innerHTML = "Damages"
      }
    }
 This works by having this function linked to te checkbox with the on-change attribute. Once the function is run it increments the counter by 1, then an if/else conditional checks if the counter is even or odd. Since a checkbox is a boolean input checking whether or not its odd or even is reliable. This story was very fun and I was happy with the solution I came up with.
 ___
 ## Third story | Style index page
  After I finished the styling of the create and edit pages I started working on the styling of the index page. After looking at the example image of what the index page should look like I got to it. When I did something quickly dawned on me. Styling the table element is really hard, but with persistence and lots of google searches I completed this part of the story. 
  
  ![IndexStyling](https://user-images.githubusercontent.com/77030485/121856599-49425780-ccba-11eb-9cc4-64cf7e0cf196.PNG)
  
  Here is a list of features along with the code that completed them.
  

  
  Once this was done I then moved on to implemented the logic  
 
