let userProfile = {
  name: '',
  goal: '',
  healthyFoods: [ /* same list */ ],
  unhealthyFoods: [ /* same list */ ],
  dietProgress: 0,
  achievements: [],
  score: 0,
  coins: 0
};

let healthyImage, unhealthyImage, additionalImage, blImage;
let inputName, inputGoal, buttonSubmit, inputFood, buttonFood;
let fitnessPlan, dietPlan, dietProgress, communityFeed, infoText;
let welcomeScreenDiv, welcomeNextButton;
let startScreenDiv, nextButton, backButton;
let showWelcome = true, showStart = false, showMain = false, showProfileEdit = false;
let achievementsDiv, resetButton, profileEditButton;

function preload() {
  healthyImage = loadImage('Helthy-removebg-preview.png');
  unhealthyImage = loadImage('unhealthy.png');
  additionalImage = loadImage('here.png');
  blImage = loadImage('bl-Photoroom.png');
}

function setup() {
  createCanvas(windowWidth, windowHeight);

  welcomeScreenDiv = createDiv('<h1>Welcome to the Fitness App</h1><h2>By - Mohammed Noor Dehalvi</h2>');
  welcomeScreenDiv.position(20, 20);
  welcomeScreenDiv.size(950, 200);
  welcomeScreenDiv.style('border', '1px solid black');
  welcomeScreenDiv.style('padding', '20px');
  welcomeScreenDiv.style('background-color', '#f9f9f9');
  welcomeScreenDiv.style('text-align', 'center');

  welcomeNextButton = createButton('Next');
  welcomeNextButton.position(width / 2 - 60, height - 80);
  welcomeNextButton.mousePressed(showStartScreen);

  startScreenDiv = createDiv('<b>Note:</b> Please enter a valid fitness goal: "Fit", "Maintain", "Muscle Gain", "Weight Loss", "Endurance", "Flexibility", "Strength", "Balance", "Agility", "Posture", "Functional Fitness", "Recovery", "Mental Wellness", "Fat", "Performance", "Body Composition".');
  startScreenDiv.position(20, 20);
  startScreenDiv.size(950, 150);
  startScreenDiv.style('border', '1px solid black');
  startScreenDiv.style('padding', '10px');
  startScreenDiv.style('background-color', '#f9f9f9');
  startScreenDiv.style('font-size', '32px');
  startScreenDiv.style('text-align', 'center');
  startScreenDiv.hide();

  nextButton = createButton('Next');
  nextButton.position(width / 2 - 60, height - 80);
  nextButton.mousePressed(showMainApp);
  nextButton.hide();

  backButton = createButton('Back');
  backButton.position(width / 2 - 60, height - 80);
  backButton.mousePressed(showStartScreen);
  backButton.hide();

  setupMainAppComponents();
}

function draw() {
  if (showWelcome) {
    background(173, 216, 230);
  } else if (showStart) {
    background(173, 216, 230);
  } else if (showMain) {
    background(173, 216, 230);

    image(healthyImage, 20, 120, 150, 150);
    image(unhealthyImage, 200, 120, 150, 150);
    image(additionalImage, 380, 120, 150, 150);
    image(blImage, 560, 120, 150, 150);

    fill(0);
    textSize(16);
    text(`Name: ${userProfile.name}`, 20, 40);
    text(`Goal: ${userProfile.goal}`, 20, 70);

    if (userProfile.dietProgress >= 100) {
      textSize(18);
      fill('green');
      text(`Congratulations! You have achieved your goal: ${userProfile.goal.toUpperCase()}!`, 20, 100);
    }

    displayCommunityFeed();
  } else if (showProfileEdit) {
    background(173, 216, 230);
  }
}

function showStartScreen() {
  showWelcome = false;
  showStart = true;
  showMain = false;
  showProfileEdit = false;
  welcomeScreenDiv.hide();
  welcomeNextButton.hide();
  startScreenDiv.show();
  nextButton.show();
  backButton.hide();
}

function showMainApp() {
  showStart = false;
  showMain = true;
  showProfileEdit = false;
  startScreenDiv.hide();
  nextButton.hide();
  backButton.show();
  showMainAppComponents();
}

function showProfileEditScreen() {
  showProfileEdit = true;
  showStart = false;
  showMain = false;
  startScreenDiv.hide();
  nextButton.hide();
  backButton.show();
  showProfileEditComponents();
}

function setupMainAppComponents() {
  inputName = createInput('');
  inputName.position(20, height - 350);
  inputName.size(200, 30);
  inputName.attribute('placeholder', 'Enter your name');
  inputName.hide();

  inputGoal = createInput('');
  inputGoal.position(20, height - 300);
  inputGoal.size(200, 30);
  inputGoal.attribute('placeholder', 'Enter your goal');
  inputGoal.hide();

  buttonSubmit = createButton('Submit');
  buttonSubmit.position(20, height - 260);
  buttonSubmit.size(100, 30);
  buttonSubmit.mousePressed(saveUserProfile);
  buttonSubmit.hide();

  inputFood = createInput('');
  inputFood.position(20, height - 220);
  inputFood.size(200, 30);
  inputFood.attribute('placeholder', 'Enter eaten food');
  inputFood.hide();

  buttonFood = createButton('Add Food');
  buttonFood.position(20, height - 180);
  buttonFood.size(100, 30);
  buttonFood.mousePressed(addFoodIntake);
  buttonFood.hide();

  fitnessPlan = createDiv('');
  fitnessPlan.position(250, height - 350);
  fitnessPlan.size(300, 80);
  fitnessPlan.style('border', '1px solid black');
  fitnessPlan.style('padding', '10px');
  fitnessPlan.hide();

  dietPlan = createDiv('');
  dietPlan.position(250, height - 250);
  dietPlan.size(300, 80);
  dietPlan.style('border', '1px solid black');
  dietPlan.style('padding', '10px');
  dietPlan.hide();

  dietProgress = createDiv('');
  dietProgress.position(250, height - 150);
  dietProgress.size(300, 30);
  dietProgress.style('border', '1px solid black');
  dietProgress.style('padding', '10px');
  dietProgress.hide();

  communityFeed = createDiv('');
  communityFeed.position(600, height - 350);
  communityFeed.size(300, 200);
  communityFeed.style('border', '1px solid black');
  communityFeed.style('padding', '10px');
  communityFeed.hide();

  infoText = createDiv('');
  infoText.position(20, height - 100);
  infoText.size(width - 40, 50);
  infoText.style('border', '1px solid black');
  infoText.style('padding', '10px');
  infoText.hide();

  resetButton = createButton('Reset Diet Progress');
  resetButton.position(20, height - 140);
  resetButton.size(150, 30);
  resetButton.mousePressed(resetDietProgress);
  resetButton.hide();

  profileEditButton = createButton('Edit Profile');
  profileEditButton.position(20, height - 100);
  profileEditButton.size(150, 30);
  profileEditButton.mousePressed(showProfileEditScreen);
  profileEditButton.hide();
  
  achievementsDiv = createDiv('');
  achievementsDiv.position(20, height - 60);
  achievementsDiv.size(width - 40, 50);
  achievementsDiv.style('border', '1px solid black');
  achievementsDiv.style('padding', '10px');
  achievementsDiv.hide();
}

function showMainAppComponents() {
  inputName.show();
  inputGoal.show();
  buttonSubmit.show();
  inputFood.show();
  buttonFood.show();
  fitnessPlan.show();
  dietPlan.show();
  dietProgress.show();
  communityFeed.show();
  infoText.show();
  resetButton.show();
  profileEditButton.show();
  achievementsDiv.show();
}

function hideMainAppComponents() {
  inputName.hide();
  inputGoal.hide();
  buttonSubmit.hide();
  inputFood.hide();
  buttonFood.hide();
  fitnessPlan.hide();
  dietPlan.hide();
  dietProgress.hide();
  communityFeed.hide();
  infoText.hide();
  resetButton.hide();
  profileEditButton.hide();
  achievementsDiv.hide();
}

function saveUserProfile() {
  userProfile.name = inputName.value();
  userProfile.goal = inputGoal.value().trim().toLowerCase();

  let fitnessText, dietText;
  switch (userProfile.goal) {
    case 'fit':
      fitnessText = 'Daily Fitness Plan: 30 minutes of cardio, 20 minutes of strength training, and 10 minutes of stretching.';
      dietText = 'Recommended Diet: Balanced diet with lean protein, vegetables, and healthy fats.';
      break;
    case 'maintain':
      fitnessText = 'Daily Fitness Plan: 20 minutes of cardio, 15 minutes of light strength training, and 10 minutes of stretching.';
      dietText = 'Recommended Diet: Balanced diet with moderate macronutrient distribution.';
      break;
    case 'muscle gain':
      fitnessText = 'Daily Fitness Plan: 45 minutes of heavy weight lifting and compound exercises.';
      dietText = 'Recommended Diet: High-protein diet with extra calories for muscle growth.';
      break;
    case 'weight loss':
      fitnessText = 'Daily Fitness Plan: 40 minutes of high-intensity interval training (HIIT) and 20 minutes of cardio.';
      dietText = 'Recommended Diet: Low-calorie diet with focus on high-fiber foods.';
      break;
    case 'endurance':
      fitnessText = 'Daily Fitness Plan: 60 minutes of continuous cardio and interval training.';
      dietText = 'Recommended Diet: High-carbohydrate diet with adequate protein for sustained energy.';
      break;
    case 'flexibility':
      fitnessText = 'Daily Fitness Plan: 45 minutes of stretching and flexibility exercises.';
      dietText = 'Recommended Diet: Balanced diet with adequate hydration and antioxidants.';
      break;
    case 'strength':
      fitnessText = 'Daily Fitness Plan: 60 minutes of heavy weight lifting and resistance training.';
      dietText = 'Recommended Diet: High-protein diet with healthy fats and carbs for muscle recovery.';
      break;
    case 'balance':
      fitnessText = 'Daily Fitness Plan: 30 minutes of balance exercises and core strengthening.';
      dietText = 'Recommended Diet: Balanced diet with focus on magnesium and potassium for muscle function.';
      break;
    case 'agility':
      fitnessText = 'Daily Fitness Plan: 30 minutes of agility drills and plyometric exercises.';
      dietText = 'Recommended Diet: Balanced diet with focus on quick-digesting carbs and hydration.';
      break;
    case 'posture':
      fitnessText = 'Daily Fitness Plan: 30 minutes of posture correction exercises and core strengthening.';
      dietText = 'Recommended Diet: Balanced diet with focus on vitamins and minerals for bone health.';
      break;
    case 'functional fitness':
      fitnessText = 'Daily Fitness Plan: 45 minutes of functional exercises including bodyweight movements.';
      dietText = 'Recommended Diet: Balanced diet with focus on nutrient-dense foods for overall health.';
      break;
    case 'recovery':
      fitnessText = 'Daily Fitness Plan: 30 minutes of gentle stretching and recovery exercises.';
      dietText = 'Recommended Diet: Balanced diet with emphasis on recovery nutrients like omega-3 fatty acids and antioxidants.';
      break;
    case 'mental wellness':
      fitnessText = 'Daily Fitness Plan: 30 minutes of mindfulness exercises and light physical activity.';
      dietText = 'Recommended Diet: Balanced diet with focus on brain-boosting foods like nuts and fish.';
      break;
    case 'fat':
      fitnessText = 'Daily Fitness Plan: 30 minutes of fat-burning exercises and strength training.';
      dietText = 'Recommended Diet: Low-fat diet with focus on high-fiber foods.';
      break;
    case 'performance':
      fitnessText = 'Daily Fitness Plan: Sport-specific training and performance enhancement exercises.';
      dietText = 'Recommended Diet: Balanced diet with focus on performance-enhancing nutrients.';
      break;
    case 'body composition':
      fitnessText = 'Daily Fitness Plan: Combination of cardio and strength training to improve body composition.';
      dietText = 'Recommended Diet: Balanced diet with focus on macronutrient distribution for optimal body composition.';
      break;
    default:
      fitnessText = 'No specific fitness plan available for this goal.';
      dietText = 'No specific diet plan available for this goal.';
  }

  fitnessPlan.html(fitnessText);
  dietPlan.html(dietText);

  updateDietProgress();

  inputName.hide();
  inputGoal.hide();
  buttonSubmit.hide();
}

function addFoodIntake() {
  let food = inputFood.value().trim().toLowerCase();
  if (userProfile.healthyFoods.includes(food)) {
    userProfile.dietProgress += 1;
  } else if (userProfile.unhealthyFoods.includes(food)) {
    userProfile.dietProgress -= 5;
  }

  userProfile.dietProgress = constrain(userProfile.dietProgress, 0, 100);
  dietProgress.html(`Diet Progress: ${userProfile.dietProgress}%`);

  inputFood.value('');
}

function resetDietProgress() {
  userProfile.dietProgress = 0;
  dietProgress.html(`Diet Progress: ${userProfile.dietProgress}%`);
}

function updateDietProgress() {
  // Update progress display, you might integrate with an AI/ML system here
  userProfile.dietProgress = 0;
  dietProgress.html(`Diet Progress: ${userProfile.dietProgress}%`);
}

function displayCommunityFeed() {
  // Example of a dynamic community feed
  let communityContent = '<b>Community Feed:</b><br>';
  let examplePosts = [
    'John: "Just completed my workout!"',
    'David: "Loving the new diet plan!"',
    'Emma: "Feeling stronger every day!"',
    'Sophia: "Can anyone recommend a good stretch routine?"'
  ];
  communityContent += examplePosts.join('<br>');
  communityFeed.html(communityContent);
}

function showProfileEditComponents() {
  hideMainAppComponents();
  inputName.show();
  inputGoal.show();
  buttonSubmit.show();
}

function displayAchievements() {
  if (achievementsDiv) {
    let achievementsText = '<b>Your Achievements:</b><br>';
    userProfile.achievements.forEach((achievement, index) => {
      achievementsText += `${index + 1}. ${achievement}<br>`;
    });
    achievementsDiv.html(achievementsText);
  }
}

function addAchievement(achievement) {
  if (achievement && !userProfile.achievements.includes(achievement)) {
    userProfile.achievements.push(achievement);
    displayAchievements();
  }
}
