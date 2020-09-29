Custom validator on Field A, that depends on a value in Field B
``` typescript
export function otherAdministratorWebsiteValidator(control: AbstractControl) {
    var invalid: boolean = false;
    if (control.parent) {
        const administeredBy: string = control.parent.get('administeredBy').value;
        const website: string = control.value || "";

        invalid = (administeredBy === SectionGroupAdministeredBy.other) && (!website.trim());
    }

	// the convention is - if it's invalid (validation FAILED), then return an object that contains the error message
	// if it is ok (validation PASSED), then return null
    return invalid ? { 'message': 'Please specify website' } : null;
}
```

Custom validator that depends on values within the current component
``` typescript
ValidateMandatoryCheckboxList(fieldCode: string): ValidatorFn {
    return (control: AbstractControl): ValidationErrors | null => {
        console.log('control current value is:', control.value);

	// If the dependent control is NOT displayed, then this control doesn't need a value
        // and so the validation will pass
        var controlDefinition = this.formDefinition.filter(f => f.fieldCode == fieldCode);
        if (controlDefinition && controlDefinition.length) {

            for (const property in control.value) {
                var propValue = control.value[property];
                if (propValue == 1 || propValue == true) {
                    return null;
                }
            }

            // Zero checkboxes were ticked, so validation failed
            return { selected: true };
        }

        // The dependent control was not displayed, so this control doesn't need a value
        return null;
    }
}
```

Password match validator
``` typescript
this.registerForm = this.formBuilder.group({
            firstName: ['', Validators.required],
            lastName: ['', Validators.required],
            email: ['', [Validators.required, Validators.email]],
            password: ['', [Validators.required, Validators.minLength(6)]],
            confirmPassword: ['', Validators.required]
        }, {
            validator: MustMatch('password', 'confirmPassword')
        });
 

export function MustMatch(controlName: string, matchingControlName: string) {
    return (formGroup: FormGroup) => {
        const control = formGroup.controls[controlName];
        const matchingControl = formGroup.controls[matchingControlName];

        if (matchingControl.errors && !matchingControl.errors.mustMatch) {
            // return if another validator has already found an error on the matchingControl
            return;
        }

        // set error on matchingControl if validation fails
        if (control.value !== matchingControl.value) {
            matchingControl.setErrors({ mustMatch: true });
        } else {
            matchingControl.setErrors(null);
        }
    }
}
```
https://jasonwatmore.com/post/2018/11/07/angular-7-reactive-forms-validation-example


Validator on the FormGroup itself
``` typescript
this.sectionGroupForm = this.formBuilder.group({
            name: '',
	// etc …
        }, { validators: identityRevealedValidator});


export const identityRevealedValidator: ValidatorFn = (control: FormGroup): ValidationErrors | null => {
  const name = control.get('name');
  const alterEgo = control.get('alterEgo');

  return name && alterEgo && name.value === alterEgo.value ? { 'identityRevealed': true } : null;
};

```
https://angular.io/guide/form-validation


Get all validation errors
https://stackoverflow.com/questions/40680321/get-all-validation-errors-from-angular-2-formgroup

``` typescript
Object.keys(this.form.controls).forEach(key => {
            const controlErrors: ValidationErrors = this.form.get(key).errors;
            if (controlErrors != null) {
                Object.keys(controlErrors).forEach(keyError => {
                    console.log('Key control: ' + key + ', keyError: ' + keyError + ', err value: ', controlErrors[keyError]);
                });
            }
        });
```

