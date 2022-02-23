** TESTING THE SOLUTION **

In order to test the solution you firstly need to add node_modules to the frontend folder (npm install).
Furthermore, you need to start up the backend server by entering npm start in the terminal.
In order to run the website, you then need to start opp the frontend by entering npm run serve in the terminal.

**Implementation**

The way I interpreted the task, is that you want an internal site where people can add tasks, mark their priorities and status (with some extra features). 
So what my solution does is that it gets the statically made API from the backend, and (only) shows the title, status and the priority of the task. I've mostly
focused on this functionality since this per se was the hardest to implement (except for the searchbar, which I will come back to).
The reason I did not implement everything that the mockup file showed is due to time limitation (4-5 hours). I therefore focused on what I though would be 
the main functionalities.

**Functionalites**

What the app does, is that it first collect all the dummydata from the API and then show it in a table. The tasks gets their priorites and status,
and you can update these by clicking on the text. Additionally, the user are able to delete a task that lets say is finished and edit a task title.
Unfortunately, I did not have time to update the database (make POST-request) in order to save these changes dynamically (but that's not all that hard work). 


**Functionalites which I wanted to implement**

The first thing I would have done is to of of course save the editing, deleting and updating of status and priorities dynamically so that the changes that the
user does actually gets made. Additionally I would have liked to implement the searchbar, since I though that it was a nice functionality to have.
The filtering and so on was less of a priority in the timeframe that I had.
