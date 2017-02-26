
var hiscoresArray = [];
var eliteLevelsArray = [];
var standardLevelsArray = [];

var websiteTwitter = "http://twitter.com/akilinosilva";
var websiteRunescape = "http://www.runescape.com/community";
var websiteTables = "http://runescape.wikia.com/wiki/Experience/Table";

var MAX = 2e8;
var MAXTOTALEXP = 5.4e9;
var MAXTOTALLEVEL = 2715;

main();

function main(){
  console.log("Fetching Username from " + websiteTwitter);
  fetchUsername();
  
  console.log("Status | Opening Browser: Accessing " + websiteRunescape);

  browser.get(websiteRunescape);
  
  console.log("Status | Opening HiScores and Fetching User Data");
  hiscores();
  
  console.log("Status | User Data Successfuly Fetched");
  console.log("Status | Accessing " + websiteTables);
  fetchLevels();
  
  console.log("Status | Calculating Levels");
  calculateLevel();
  
}

function fetchUsername(){
  browser.get(websiteTwitter);
  myTweets = $(".my-tweet");
  tweetText = $(".tweet-text");
  
  var i;
  
  for(i = 0; i < tweetText.size();i++){
    tweetLine = tweetText.eq(0).text();
    if(tweetLine.startsWith("Fetch Hiscores")){
      username = tweetLine.slice(15);
    }
  }
}

function hiscores(){
  $(".main-nav__heading").withText("Community").moveTo().waitForExistence().withText("Community");
  $(".main-nav__group-item-link").withVisibleText("HiScores").click();
  
  getHiscores(username);
}

function clearHiscores(){
  playerNameField = $("input~ input+ .text");
  if(playerNameField !== null){
    playerNameField.clear();
  }
}

function getHiscores(username){
  clearHiscores();
  
  playerNameField = $("input~ input+ .text").type("");
  insertName = playerNameField.type(username);
  $("input").rightOf(playerNameField).click();
  
  getProfile(username);
}

function strcmp(a, b) {
    a = a.toString(), b = b.toString();
    for (var i=0,n=Math.max(a.length, b.length); i<n && a.charAt(i) === b.charAt(i); ++i);
    if (i === n) return 0;
    return a.charAt(i) > b.charAt(i) ? -1 : 1;
}

function getProfile(username){
  
  //levelHeader = $("th").withText("Level");
  playerHeader = $("th").withText("Player");
  cells = $(".tableWrap a");
  
  player = cells.below(playerHeader).withText(username);

  player.click();
  //cells.below(levelHeader).rightOf(player).waitForExistence().text();
  
  getValues();
}

function Skill(skillName,rank,xptotal,level){
  this.skillName = skillName;
  this.rank = rank;
  this.xptotal = xptotal;
  this.level = level;
}

function getName(index){
  return skillIcons.eq(index-1).attr("title");
}

function getRank(index){
  return rankValue.and($("tr:nth-child("+ index +") a")).leftOf(skillIcons).text();
}

function getXPTotal(index){
  return totalExpValue.and($("tr:nth-child(" + index + ") a")).leftOf(skillIcons).text().replace(",","");
}

function getLevel(index){
  line = levelValue.and($("tr:nth-child(" + index + ") a")).leftOf(skillIcons).text();
  if(line.indexOf(',') > -1){
    line = line.replace(",","");
  }
  
  return line;
}

function getValues(){
  rank = $("th").withText("Rank");
  totalExp = $("th").withText("Total XP");
  level = $("th").withText("Level");
  
  cells = $(".tableWrap a");
  
  skillIcons = $(".skillIcons div");
  
  rankValue = cells.below(rank);
  totalExpValue = cells.below(totalExp);
  levelValue = cells.below(level);
  
  for(index = 1; index <= skillIcons.size();index++){
    skill = new Skill(getName(index),getRank(index),parseInt(getXPTotal(index)),parseInt(getLevel(index)));
    hiscoresArray.push(skill);
  }
}

function StandardLevels(level,xp){
  this.level = level;
  this.xp = xp;
}

function EliteLevels(level,xp){
  this.level = level;
  this.xp = xp;
}

function fetchLevels(){
  browser.get(websiteTables);
  
  console.log("Status | Fetching XP Tables");
  
  standardSkillsText = $("#toc+ h2");
  levels100To120Text = $("h3:nth-child(6)");
  eliteSkillsText = $("#INCONTENT_WRAPPER+ h2");
  levels121To150Text = $("h3:nth-child(16)");
  levels121To126 = $(".alternating-rows+ h3");
  
  for(j = 0; j < 3; j++){
    level = $("th").withText("Level").above(levels100To120Text).eq(j);
    XP = $("th").withText("XP").eq(j); //gave so many problem.

      for(i = (j*25) + 1; i < ((j+1)*25) + 1 ; i++){

        variableLevel = $("td").withText(i).above(eliteSkillsText).below(level);
        variableExp = $("td").rightOf(variableLevel).below(XP);
        
        standardLevel = new StandardLevels(variableLevel.text(),variableExp.text().replace(",",""));
        standardLevelsArray.push(standardLevel);
        
        //console.log(variableLevel.text() + variableExp.text().replace(",",""));
      }
  }

  for(i = 76; i < 100; i++){
      
      level = $("th").withText("Level").below(standardSkillsText).eq(2);
      XP = $("th").withText("XP").eq(3); //gave so many problem.

      variableLevel = $("td").withText(i).below(level).above(levels100To120Text);
      variableExp = $("td").rightOf(variableLevel).below(XP);
      
      standardLevel = new StandardLevels(variableLevel.text(),variableExp.text().replace(",",""));
      standardLevelsArray.push(standardLevel);
      
      //console.log(variableLevel.text() + variableExp.text().replace(",",""));
  }
  
  
  for(j = 0; j < 3; j++){
    level = $("th").withText("Level").above(levels121To126).below(levels100To120Text).eq(j);
    XP = $("th").withText("XP").eq(j); //gave so many problem.

      for(i = 100 + (j*7); i < 100 + ((j+1)*7) ; i++){

        variableLevel = $("td").withText(i).above(eliteSkillsText).below(level);
        variableExp = $("td").rightOf(variableLevel).below(XP);
        
        standardLevel = new StandardLevels(variableLevel.text(),variableExp.text().replace(",",""));
        standardLevelsArray.push(standardLevel);
        
        //console.log(variableLevel.text() + variableExp.text().replace(",",""));
      }
  }
  


  for(i = 121; i < 127 ; i++){

    level = $("th").withText("Level").above(eliteSkillsText).below(levels121To126);
    XP = $("th").withText("XP").above(eliteSkillsText).below(levels121To126); //gave so many problem.
      
    variableLevel = $("td").withText(i).above(eliteSkillsText).below(levels121To126);
    variableExp = $("td").rightOf(variableLevel).below(XP);
    
    standardLevel = new StandardLevels(variableLevel.text(),variableExp.text().replace(",",""));
    standardLevelsArray.push(standardLevel);
    
    //console.log(variableLevel.text() + variableExp.text().replace(",",""));
  }


  
  for(j = 0; j < 4; j++){
    level = $("th").withText("Level").below(eliteSkillsText);
    XP = $("th").withVisibleText("XP").below(eliteSkillsText).eq(j);

      for(i = (j*30) + 1; i < ((j+1)*30) + 1 ; i++){

        variableLevel = $("td").withText(i).below(level);
        variableExp = $("td").rightOf(variableLevel).below(XP);
        
        eliteLevel = new EliteLevels(variableLevel.text(),variableExp.text().replace(",",""));
        eliteLevelsArray.push(eliteLevel);
        
        //console.log(variableLevel.text() + variableExp.text().replace(",",""));
      }
  }
  
  for(j = 0; j < 3; j++){
    level = $("th").withText("Level").below(levels121To150Text);
    XP = $("th").withVisibleText("XP").below(eliteSkillsText).eq(j);
    
    for(i = 120 + (j*10) + 1; i < 120 + ((j+1)*10) + 1 ; i++){
      
      variableLevel = $("td").withText(i).below(level);
      variableExp = $("td").rightOf(variableLevel).below(XP);
    
      eliteLevel = new EliteLevels(variableLevel.text(),variableExp.text().replace(",",""));
      eliteLevelsArray.push(eliteLevel);
    
      //console.log(variableLevel.text() + variableExp.text().replace(",",""));
    }
  }
  console.log("Status | XP Tables Successfuly Fetched");
}

function format(num){
    var n = num.toString(), p = n.indexOf('.');
    return n.replace(/\d(?=(?:\d{3})+(?:\.|$))/g, function($0, i){
        return p<0 || i<p ? ($0+',') : $0;
    });
}

function calculatePercentage(currentLevel, nextLevel,currentExp){
  
  value = (100 - ((nextLevel-currentExp)*100)/(nextLevel-currentLevel)).toString();
  indexOfSeparator = value.indexOf(".");
  percentage = value.substring(0,indexOfSeparator+3) + "%";
  
  return percentage;
}

function sleep(milliseconds) {
  var start = new Date().getTime();
  for (var i = 0; i < 1e7; i++) {
    if ((new Date().getTime() - start) > milliseconds){
      break;
    }
  }
}

function calculateLevel(){
  
  console.log("\r\n");
  console.log("'" + username + "' - Levels Calculated \r\n");
  
  sleep(1000);

  for(index = 0; index < 28; index++){
    if(hiscoresArray[index].xptotal == MAXTOTALEXP){
      console.log(hiscoresArray[index].skillName + ": Max Total Level and Experience Reached! " + format(MAXTOTALEXP));
    }else if(hiscoresArray[index].skillName == "Overall"){
      console.log("Overall: Current Total Level is " + hiscoresArray[index].level);
    }else if(hiscoresArray[index].xptotal == MAX){
      console.log(hiscoresArray[index].skillName + ": Max Level and Experience Reached! " + format(MAX));
    }else if(parseInt(hiscoresArray[index].level) == 120){
      higherThan120(index);
    }else if(parseInt(hiscoresArray[index].level) == 99){
      higherThan99(index);
    }else if(parseInt(hiscoresArray[index].level) < 99){
      remainingLevels(index);
    }else if(isNaN(hiscoresArray[index].level)){
      console.log(hiscoresArray[index].skillName + ": Data not found.");
    }
  }
}

function remainingLevels(index){
  
  if(hiscoresArray[index].skillName == "Invention"){
    for(i = 0; i < 99; i++){
      hiscoresXP = hiscoresArray[index].xptotal;
      percentage = calculatePercentage(parseInt(eliteLevelsArray[i].xp),parseInt(eliteLevelsArray[i+1].xp),hiscoresXP);
      
      if(hiscoresXP <= parseInt(eliteLevelsArray[i].xp)){
        
        console.log(hiscoresArray[index].skillName + ": Current Level is " + i + " | Remaining XP to next Level: " + format(parseInt(eliteLevelsArray[i].xp)-(hiscoresXP)) + " XP | " + percentage + " Completed");
        break;
      }
    }
  }else{
    for(j = 0; j < 99; j++){
      hiscoresXP = hiscoresArray[index].xptotal;
      percentage = calculatePercentage(parseInt(standardLevelsArray[j].xp),parseInt(standardLevelsArray[j+1].xp),hiscoresXP);
      if(hiscoresXP <= parseInt(standardLevelsArray[j].xp)){
        console.log(hiscoresArray[index].skillName + ": Current Level is " + j + " | Remaining XP to next Level: " + format(parseInt(standardLevelsArray[j].xp)-(hiscoresXP)) + " XP | " + percentage + " Completed");
        break;
      }
    }
  }
}

function higherThan99(index){
  if(hiscoresArray[index].skillName == "Invention"){
    for(i = 99; i < 121; i++){
      hiscoresXP = hiscoresArray[index].xptotal;
      percentage = calculatePercentage(parseInt(eliteLevelsArray[i-1].xp),parseInt(eliteLevelsArray[i].xp),hiscoresXP);
      if(hiscoresXP <= parseInt(eliteLevelsArray[i].xp)){
        console.log(hiscoresArray[index].skillName + ": Current Level is " + i + " | Remaining XP to next Level: " + format(parseInt(eliteLevelsArray[i].xp)-(hiscoresXP)) + " XP | " + percentage + " Completed");
        break;
      }
    }
  }else{
    for(j = 99; j < 121; j++){
      hiscoresXP = hiscoresArray[index].xptotal;
      percentage = calculatePercentage(parseInt(standardLevelsArray[j-1].xp),parseInt(standardLevelsArray[j].xp),hiscoresXP);
      if(hiscoresXP <= parseInt(standardLevelsArray[j].xp)){
        console.log(hiscoresArray[index].skillName + ": Current Level is " + j + " | Remaining XP to next Level: " + format(parseInt(standardLevelsArray[j].xp)-(hiscoresXP)) + " XP | " + percentage + " Completed");
        break;
      }
    }
  }
}

function higherThan120(index){
  
  if(hiscoresArray[index].skillName == "Invention"){
    for(i = 120; i < 151; i++){
      hiscoresXP = hiscoresArray[index].xptotal;
      percentage = calculatePercentage(parseInt(eliteLevelsArray[i-1].xp),parseInt(eliteLevelsArray[i].xp),hiscoresXP);
      if(hiscoresXP <= parseInt(eliteLevelsArray[i].xp)){
        console.log(hiscoresArray[index].skillName + ": Current Level is " + i + " | Remaining XP to next Level: " + format(parseInt(eliteLevelsArray[i].xp)-(hiscoresXP)) + " XP | " + percentage + " Completed");
        break;
      }
    }
  }else{
    for(j = 120; j < 127; j++){
      hiscoresXP = hiscoresArray[index].xptotal;
      percentage = calculatePercentage(parseInt(standardLevelsArray[j-1].xp),parseInt(standardLevelsArray[j].xp),hiscoresXP);
      if(hiscoresXP <= parseInt(standardLevelsArray[j].xp)){
        console.log(hiscoresArray[index].skillName + ": Current Level is " + j + " | Remaining XP to next Level: " + format(parseInt(standardLevelsArray[j].xp)-(hiscoresXP)) + " XP | " + percentage + " Completed");
        break;
      }
    }
  }
}
