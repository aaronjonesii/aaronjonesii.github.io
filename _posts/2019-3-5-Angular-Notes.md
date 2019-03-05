---
layout: post
title: Angular Notes
image: https://user-images.githubusercontent.com/18272237/53821956-c65bbf80-3f3c-11e9-9d78-001a377abd0f.png
---
Information on Angular w/examples of common features.


#### Create an Object Class
Create a class in its own file for an object to define its properties.
```javascript
// src/app/task.ts
export class Task {
    id = number;
    name = string;
    owner = string;
}
```
Return to your component and import the objects class to use the properties of the class.
```javascript
// src/app/home
import { Component, OnInit } from '@angular/core';
import { Task } from '../task';

@Component({
  selector: 'anon-tasks',
  templateUrl: './tasks.component.html',
  styleUrls: ['./tasks.component.css']
})
export class TasksComponent implements OnInit {
  task: Task = {
    id: 1,
    name: 'First Task',
    owner: 'Admin'
  };

  constructor() { }

  ngOnInit() {
  }

}
```

Show the task object:
```html
<h2>{{task.name}} Details</h2>
<div><span>ID: </span>{{task.id}}</div>
<div><span>Owner: </span>{{task.owner}}</div>
```

Use Two-way binding:
```html
<div>
  <label>Task name:
    <input [(ngModel)]="task.name" placeholder="name">
  </label>
</div>
```
`[(ngModel)]` is Angular's two-way data binding syntax.


***
[Angular Docs](https://angular.io/docs)


