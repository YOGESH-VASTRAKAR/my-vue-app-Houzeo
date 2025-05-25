This project follows a modular structure with a main App.vue importing three key components:

  1. HeaderComponent.vue
  2. MapComponent.vue
  3. PropertyListings.vue

How Components Work Together

 App.vue acts as the root component, orchestrating data flow :

 my-vue-app/  
  ├── src/  
  │   ├── assets/          # svg icon
  │   ├── components/      # HeaderComponent.vue  # MapComponent.vue  # PropertyListings.vue
  │   ├── App.vue          # Main Vue component  
  │   └── main.js          # Vue app entry point  
  ├── public/              # images
  ├── .gitignore           # Files to ignore in Git  
  ├── package.json         # Project dependencies  
  └── README.md            # This file  

How to Run the project and Builds for production 

  npm run dev          # Starts development server
  npm run build        # Builds for production

Contact

  Author: Yogesh Vastrakar
  Email: yogeshvastrakar.yv@gmail.com
  LinkedIn: https://www.linkedin.com/in/yogesh-vastrakar/
  Portfolio: https://yv-my-portfolio.netlify.app/
  
Live Demo 
🌐 https://houzeo-vue-task.netlify.app/
