# NBA-Team-Info-Project
// Data NBA Table Instance Variables
var conference = getColumn("NBA Teams", "Conference");
var division = getColumn("NBA Teams", "Division");
var team = getColumn("NBA Teams", "Team");
var location = getColumn("NBA Teams", "Location");
var arena = getColumn("NBA Teams", "Arena");
var arenaCap = getColumn("NBA Teams", "Arena capacity");
var year = getColumn("NBA Teams", "Year joined to the NBA");
var champWins = getColumn("NBA Teams", "Championship wins");
var images = getColumn("NBA Teams", "Image");

// Instance Lists
var divisionList = [];
var teamList = [];
var locList = [];
var arenaList = [];
var arenaCapList = [];
var yearList = [];
var champList = [];
var imageList = [];


dropdown("confDropDown", "Choose a Conference", "Western", "Eastern");  // Creates a dropdown for the Conference option Input.
setPosition("confDropDown", 65, 150, 200, 50);                          // Adjusts the appearance of the dropdown.
onEvent("confDropDown", "change", function(event) {                     // Checks to see if the Conference Dropdown has been changed to a different option.
  var confInput = getText("confDropDown");                              // If it was changed, it would store the text of the option into the variable, input, and
  showDivDropDown(confInput);                                           // Calls the showDivDropDown function with the input as an argument.
});                                                                     



function showDivDropDown(conf_Input) {                                  // The function creates a new dropdown, divDropDown, that shows the divisions avaliable for specified conference using the conf_Input parameter.
  dropdown("divDropDown", "Choose a Division");                         // Creates a new Dropdown for the Division Options.
  setPosition("divDropDown", 65, 225, 200, 50);                         // Sets positions for the Division Dropdown.
  divisionList = [];                                                    // Initalizing the list.
  
  for(var i = 0; i < conference.length; i++) {                          // The for loop goes through the length of the Conference column from the NBA Team Data Table
    if(conference[i] == conf_Input) {                                   // to check if any conference at current index is equal to the conf_Input.
      appendItem(divisionList, division[i]);                            // If true, it will be appended onto the initalized list
    }                                                                   
  }                                                                     
  removeMultiples(divisionList);                                        // Calls the removeMultiples function.
  setProperty("divDropDown", "options", divisionList);                  // Sets the property of the options from the divDropDown dropdown with the organized list of avaliable divisions.

  onEvent("divDropDown", "change", function(event) {
    button("nextButton", "Show Results");                               // Creates a button that, when pressed, opens to a new screen and outputs the results
    setPosition("nextButton", 100, 300, 125, 50);                       // Adjusting the button position.
    
    onEvent("nextButton", "click", function(event) {
      setScreen("InputScreen");
      var divInput = getText("divDropDown");   
      appendColumns(conf_Input, divInput);     
    });
  });
}
function removeMultiples(list) {                                        // The function allows the list parameter to be more organized by removing any multiple items present within the list.      
  insertItem(list, 0, "Choose a Division");
  for(var j = 0; j < list.length; j++) {                                // It loops through the length of the list parameter and checks if the first two elements are the same.
    var curElement = list[j];                                           // Grabs the current element.
    var nextElement = list[j+1];                                        // Grabs the next element.  

    if(curElement == nextElement) {                                     // if both are the same, it will remove the next element, and resets the index to repeat again until all of the elements 
      removeItem(list, j);                                              // within the list parameter are organized.
      j = 0;
    }
  }
  return list;                                                          // Returns the fully organized, non-multiple list.
}
function appendColumns(conferenceInput, divisionInput) {
  for(var j = 0; j < conference.length && j < division.length; ++j) {
    if(conference[j] == conferenceInput && division[j] == divisionInput) {
      appendItem(teamList, team[j]);
      appendItem(locList, location[j]);
      appendItem(arenaList, arena[j]);
      appendItem(arenaCapList, arenaCap[j]);
      appendItem(yearList, year[j]);
      appendItem(champList, champWins[j]);
      appendItem(imageList, images[j]);    
    }
  }
  insertItem(teamList, 0, "Select Team for More Info");
  dropdown("teamName", "Team Dropdown");
  setPosition("teamName", 20, 180, 275, 50);  
  setProperty("teamName", "options", teamList);
  
  onEvent("teamName", "change", function(event) {
    button("resultButton", "Show NBA Information");
    setPosition("resultButton", 20, 240, 275, 50);  
    
    onEvent("resultButton", "click", function(event) {
      var result = getText("teamName");
      createUI(result);
      setScreen("infoScreen");
      
    });
  });
}
function createUI(text_Name) {
  var i = teamList.indexOf(text_Name) - 1;
  setProperty("teamNamer", "text", text_Name);
  setProperty("teamLoc", "text", locList[i]);
  setProperty("teamArena", "text" , arenaList[i]);
  setProperty("arenaCapital", "text", arenaCapList[i]);
  setProperty("teamYear", "text", yearList[i]);
  setProperty("teamChamp", "text", champList[i]);
  setProperty("teamImage", "image", imageList[i]);
  
}
