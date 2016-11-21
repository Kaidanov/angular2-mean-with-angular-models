# angular2-mean-with-angular-models


First Command Line run to run node.js server
Each change needs to restart
```
npm run build  
```

Second Command Line to run the angular client ( views of node for now)
```
npm start
```

To use MongoDB after installation add packages
```
	Ø npm install --save mongoose
	Ø npm install mongoose-unique-validator   (--this one is for unique fields)
```

For running MongoDB server
```
cd E:\MongoDB\Server\3.2\bin
E:\MongoDB\Server\3.2\bin>mongod.exe --dbpath "e:\mongodb\db"
```

You can also see the port the mongo is working on and init in node.js accordingly 
//connecting to mongodb
//from running server gather the port
//creating the db if not existing
```javascript
mongoose.connect('localhost:27017/node-angular');
```

For running MongoDB client ( just to check the validity of our data - usually will be accessed trhoough the node.js code)
```
cd E:\MongoDB\Server\3.2\bin
E:\MongoDB\Server\3.2\bin>mongo.exe 
```
Can check through the client using those commands :
```
	Ø use node-angular  //swithes to dn node-angular
	Ø show collections //instead of tables
	Ø db.messages.find() //gets the data of the tables/collections
```

Example of getting data from MongDB in Node.js 

..\routes\app.js
```javascript
router.get('/', function (req, res, next) {
    User.findOne({}, function(err, doc){
       if(err)
       {
           return res.send('Error!');
       }
       res.render('node', {email: doc.email});
    });
});
```


**_Angular2_**  
**1.Property Binding**   
Connecting to  
- DOM Properties (e.g. value, hidden)
- Directive Properties (e.g. ngStyle
- Component Properties
```
 [property] = "expression"
```
For instance 
``` 
  in Typescript
  @Input('Alias') propertyName
  
  In HTML
  <my-component [propertyName] = "expression"> </my-component>
```

**2.Event Binding**  
Connecting to   
- User/DOM events (e.g. click, mouseover..)
- Directive Events
- Components Events
```
 (event) = "expression"
```
For Instance
``` 
  in Typescript
  @Output('Alias') eventName
  
  In HTML
  <my-component (eventName) = "expression"> </my-component>
```

Directives use 'selector' to let Angular2 know which parts of the HTMLl code represent instructions
like ``*ngIf``` or  ```my-component```
``` HTML
<section  *ngIf = "condition">
<h1> Hi </h1>
</section>

<my-component> </my-component>
```


**Creating dynamic color toggle **  
In the compnenet define a member color with initial color
and at the html add this
``` HTML
<article class="panel panel-default" [ngStyle]="{backgroundColor:color}" (mouseenter)="color = 'green'" (mouseleave)="color = 'red'">
</article>
```

[ngStyle] - gives to backgroundColor the initialized value of the member of the component named color  
(mouseenter) (mouseleave) - events changing the color by event and therefore updating the backgroundColor  


** Set value reference to local control and pass it to a function back to Component **  
 #input is sending it's value from html to onSave function without extra parameters. 
``` Html
<div class="col-md-8 col-md-offset-2">
    <div class="form-group">
        <label for="content" >
            <input type="text" id="content" class="form-control" #input>
        </label>
    </div>
    <button class="btn btn-primary" type="submit" (click)="onSave(input.value)">Save</button>
</div>
```
In TS we know what we are waiting for - value of type string. 
``` Javascript
import {Component} from "@angular/core";
/**
 * Created by Tzvika on 11/21/2016.
 */
@Component({
    selector: 'app-message-input',
    templateUrl : './message-input.component.html'
})
export class MessageInputComponent{
    onSave(value:string){
       console.log(value);
   }
}
```


** Services **
Used for reuse in different components.
We can provide service on the app level or more specificaly per component when needed.


** Using Forms **  
Create a wrapper of a form , define a local variable inside and reffer it to the object of form created by angular for you
``` HTML 
 <div class="col-md-8 col-md-offset-2">
    <form (ngSubmit)="onSubmit(f)" #f="ngForm">
        <div class="form-group">
            <label for="content" >Content</label>
             <input
                     type="text"
                     id="content"
                     class="form-control"
                     ngModel
                     name="content"
                     required>

        </div>
        <button class="btn btn-primary" type="submit" >Save</button>
    </form>
</div>
```
Inside the component
``` Typescript
export class MessageInputComponent{
    constructor(private messageService: MessageService){}

    onSubmit(form: NgForm){
      const message = new Message(form.value.content, "tzvika");
      this.messageService.addMessage(message);
      form.resetForm();
   }
}
```
If the input is empty submitted it will find the added by angular classes and show it with red borders.
``` CSS
	input.ng-invalid.ng-touched {
	  border: 1px solid red;
	}

```

** Routing **
Setting custom routing   
1. pathMatch: 'full' making the interpretation of the full path . good for empty path  
2. setting '/messages' with a slash will take you to the domain_path/your_route and not relative  
3. for module to take our routes we need to export them, therefore - export const routing 
4. 

``` Javascript
import {Routes, RouterModule} from "@angular/router";

import {MessagesComponent} from "./messages/messages.component";
import {AuthenticationComponent} from "./auth/authentication.component";

/**
 * Created by Tzvika on 11/21/2016.
 */
const APP_ROUTES: Routes = [
    //pathMatch only matches the really empty route
    {path : '' , redirectTo: '/messages' , pathMatch: 'full' },  // goto to localhost (to your domain) and not to #
    {path : 'messages' , component: MessagesComponent },
    {path : 'auth' , component: AuthenticationComponent }
];

//exporting for getting our custom routing
export const routing = RouterModule.forRoot(APP_ROUTES);
```

** The HTML Component part **  
1. routerLinkActive="active" - sets the link inside the li as active, adding style automatically
2. [routerLink]="['/messages']"  - sets the same way as in routiing full path relative to your domain
``` HTML
		<header class="row">
                    <nav class="col-md-8 col-md-offset-2">
                        <ul class="nav nav-pills">
                           <!-- (li>a)*2) -->
                           <li routerLinkActive="active"><a [routerLink]="['/messages']" >Messenger</a></li>
                           <li routerLinkActive="active"><a [routerLink]="['/auth']">Authentication</a></li>
                        </ul>
                    </nav>
                </header>
```
