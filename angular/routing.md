https://medium.com/@shairez/angular-routing-a-better-pattern-for-large-scale-apps-f2890c952a18

Manually Route
First you need to import the angular router :
import {Router} from "@angular/router"
Then inject it in your component constructor :
constructor(private router: Router) { }
And finally call the ..navigate method anywhere you need to "redirect" :
this.router.navigate(['/your-path'])
You can also set some parameters on your route, like user/5 :
this.router.navigate(['/user', 5])

this.router.navigate(['/entries/new/entry-form'], {
            queryParams:
            {
                exhibitorId: this.exhibitorId,
                classId: this.selectedClass.id
            }
        });




From <https://stackoverflow.com/questions/47133610/angular-4-manual-redirect-to-route> 


Get Route parameters form URL
Given the route: /entries/:competitionEntryId
https://stackoverflow.com/questions/40275862/how-to-get-parameter-on-angular2-route-in-angular-way


import { Router, ActivatedRoute } from '@angular/router';

constructor(
	
        private activatedRoute: ActivatedRoute) {
 }


this.activatedRoute.queryParams.subscribe(params => {
            const competitionEntryId: number = this.route.snapshot.paramMap.get('competitionEntryId');
}




Get Route parameters form QueryString
Accessing Query Param Values
Now that we know how to pass-in optional query parameters to a route, let’s see how to access these values on the resulting routes. The ActivatedRoute class has a queryParams property that returns an observable of the query parameters that are available in the current url.

Given the following route URL:

http://localhost:4200/products?order=popular
We can access the order query param like this:

// ...
import { ActivatedRoute } from '@angular/router';
import 'rxjs/add/operator/filter';

@Component({ ... })
export class ProductComponent implements OnInit {
  order: string;
  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.route.queryParams
      .filter(params => params.order)
      .subscribe(params => {
        console.log(params); // {order: "popular"}

        this.order = params.order;
        console.log(this.order); // popular
      });
  }
}


Declaratively Route with Parameters
<a [routerLink]="['/user/bob']" [queryParams]="{debug: true}">
