# CSharpLiveProject
This repository is dedicated solely to highlighting the contributions I made to project I did with my Code Bootcamp. During this time I worked on a CMS (Content Managment System) for local theather in Portland under by code bootcamp's sister company [Prosper IT Consulting](https://www.learncodinganywhere.com/ProsperITConsulting). The CMS itself was made with ASP .Net MVC and Entity Framework. This readme document will talk about my specific contributions.
___

  My first story I automated the process of counting the number of contributing devs to theater CMS with pure JS. I did this by counting the p tag, because it was only being used for names. So I wrote a small piece of script that stores the amount of p tags on the document in a varible and places the value next to the H2 tag.  

This is the script itself

    function countName() {
        let count = document.getElementsByTagName("p").length; 
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
  ## Second Story | Style create and edit pages
    Now it was time to add some styling to the create and edit pages based of a provided image. 
  I turned create and edit pages into a replica of the images with bootstrap and this is how it turned out.
    
   ![createeditStyle](https://user-images.githubusercontent.com/77030485/121847673-8f91b980-ccae-11eb-8c77-c849f1755db0.PNG)
  
  Implementing this lovely design was not the end of this story. Now I had to find to find a way to update the header text where it says notes to change to "damages" when the checkbox was click. 
  
  ### This was my solution
  
    let Counter = 0;
    function checkLink() {
      Counter += 1;
      if (Counter % 2 === 0) {
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
  
  Here is a list of features the index page has. 
  
  -Depending on whether or not the rental is damaged the code will display a red X or a green checkmark *by virtue of Font Awesome*
  
  -If the desc. text overflows an ellipses appears
  
  -an ellipses drop down shows up upon hovering on a row
  
___
## Fourth Story | Sorting logic

  After completing the the styling of the index page I now had to implement the logic for sorting. This was pretty straight forward.. or so I thought. The challenging aspect of this story was the fact that the page was not suppose to reload upon resorting. So I decided to use AJAX to accomplish this. 
  
  ![IndexSort](https://user-images.githubusercontent.com/77030485/121978925-d59e5a00-cd4e-11eb-9d9e-af97474c7180.PNG)
  
  ### Here's how it works
  
        <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
          <a class="dropdown-item" onclick="sortFun('default'); changeNXS();">No Extra Sorting</a>
          <a class="dropdown-item" onclick="sortFun('damage_rentals'); changeDR();">List Damaged Rentals</a>
          <a class="dropdown-item" onclick="sortFun('damage_rentals_desc'); changeUR();" >List Undamaged Rentals</a>
          <a class="dropdown-item" onclick="sortFun('alpha_rentals'); changeA();" >List Alphabetical</a>
          <a class="dropdown-item" onclick="sortFun('alpha_rentals_desc'); changeRA();">List Reverse Alphabetical</a>
        </div>
  The drop down list is link to an ajax function sortFun() and another function that changes the button title. 
  Let's take a look at how the AJAX function works.
  
    function sortFun(sortOrder) {
      console.log("This hit");
      $.ajax({
        type: "POST",
        url: "@Url.Action("PartialTable")",
        data: {sortOrder : sortOrder},
        success: function (data) {
          document.getElementById("checkBody").innerHTML = data;
        }
      });
      console.log(sortOrder);
    }
  This is the funciton itself. It works by taking a string parameter called sortOrder then posting the argument to the controller name PartialTable. Then from this action method the sortOrder var is ran through an if else block to find its match. 
  
  public PartialViewResult PartialTable(string sortOrder)
        {
            let rentalHistories = from s in db.RentalHistories
                                  select s;
            if (sortOrder === "default")
            {

            }
            else if (sortOrder === "damage_rentals")
            {
                rentalHistories = rentalHistories.OrderByDescending(s => s.RentalDamaged);
            }
            else if (sortOrder === "damage_rentals_desc")
            {
                rentalHistories = rentalHistories.OrderBy(s => s.RentalDamaged);
            }
            else if (sortOrder === "alpha_rentals")
            {
                rentalHistories = rentalHistories.OrderBy(s => s.Rental);
            }
            else if (sortOrder === "alpha_rentals_desc")
            {
                rentalHistories = rentalHistories.OrderByDescending(s => s.Rental);
            }
            return PartialView("_IndexTable", rentalHistories);
        }
  After reordering the data using LINQ. This function returns the partial view name _IndexTable and a object model rentalHistories.  
  
      @foreach (var item in Model)
    {
      <tr id="Rental_History-Index--hover-row">
        <td id="Rental_History-Index--BoolIcon">
          @if (item.RentalDamaged == true)
          {
            <i id="Rental_History-Index--crossmarkclass" class="fas fa-times-circle"></i>
          }
          else
          {
            <i id="Rental_History-Index--checkmarkclass" class="fa fa-check-circle" aria-hidden="true"></i>
          }
        </td>
        <td style="border-top: solid black 2px; width: 200px;">
          <button type="button" class="btn btn-dark">@Html.DisplayFor(modelItem => item.Rental)</button>
        </td>
        <td id="Rental_History-Index--TextOverFlow">
          @Html.DisplayFor(modelItem => item.DamagesIncurred)
        </td>
        <td id="Rental_Style-Index--bordertopWidth">
          <div class="dropdown mydropdowncss">
            <button class="btn btn-dark" type="button" id="dropdownMenuButton1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              <i class="fa fa-ellipsis-v" aria-hidden="true"></i>
            </button>
            <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
              <li> <a class="dropdown-item" href="@Url.Action("Edit", new { id = item.Id })"><i style="padding-right: 5px;" class="fa fa-edit" aria-hidden="true"></i>Edit </a></li>
              <li> <a class="dropdown-item" href="@Url.Action("Details", new { id = item.Id })"> <i style="padding-right: 5px;" class="fa fa-info" aria-hidden="true"></i>Details </a></li>
              <li><div class="dropdown-divider"></div></li>
              <li> <a class="dropdown-item" href="@Url.Action("Delete", new { id = item.Id })"><span style="color: red;"><i style="padding-right: 5px;" class="fa fa-trash" aria-hidden="true"></i>Delete</span> </a></li>
            </ul>
          </div>
        </td>
      </tr>
    }
  *Above is the partial view code*
  After the action method is done doing its work the AJAX script runs the success function.
  
    success: function (data) {
          document.getElementById("checkBody").innerHTML = data;
        }
  This function takes in a parameter named "data" which has access to the reordered model and the partial view name. It then replaces the default partial view.
  
    <tbody id="checkBody">
    @Html.Partial("_IndexTable")
    </tbody>
    
  With the new one.  
  
    document.getElementById("checkBody").innerHTML = data;
    
  ___
  ## Wrap up on what I learned
  
  This project was so much and a real challenge. These are skill I engaged. 
  
  Solidify my understanding of git verbs like 
  commit 
    A snapshot of the current state of the code in ones local repo
  Push 
    Takes all of the code and uploads it to the ones remote repo
  Clone/pull
    cloning is when you copy the remote repo and pulling is when you get the changes from the remote repo after cloning. 
  Pull request
    A request made to merge your changes into the main remote repo
  
  

Familiarize myself with agile and SCRUM methodologies

participated in daily stand up, and weekly sprint retrospectives and planning

used Bootstrap to make responsive designs that work on many different view ports.

used Javascript to automate tasks.

used Ajax to render pages asynchronously.

Created entity models by using entity framework.

Scaffolded CRUD pages

Used MVC design pattern to render pages dynamically.
___
