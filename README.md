custom select

import { Component, forwardRef, Input, OnInit } from '@angular/core';
import { ControlValueAccessor, NG_VALUE_ACCESSOR } from '@angular/forms';

@Component({
  selector: 'app-custom-select',
  templateUrl: './custom-select.component.html',
  styleUrls: ['./custom-select.component.css'],
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: forwardRef(() => CustomSelectComponent),
      multi: true
    }
  ]
})
export class CustomSelectComponent implements OnInit, ControlValueAccessor {
  @Input() label: string;
  @Input() options: any[];
  @Input() required: boolean;

  selectedOption: any;
  disabled = false;

  onChange: any = () => {};
  onTouch: any = () => {};

  constructor() { }

  ngOnInit(): void {
  }

  set selectedValue(value: any) {
    this.selectedOption = value;
    this.onChange(value);
    this.onTouch();
  }

  writeValue(value: any): void {
    this.selectedOption = value;
  }

  registerOnChange(fn: any): void {
    this.onChange = fn;
  }

  registerOnTouched(fn: any): void {
    this.onTouch = fn;
  }

  setDisabledState?(isDisabled: boolean): void {
    this.disabled = isDisabled;
  }
}


custom select html

<mat-form-field>
  <mat-label>{{ label }}</mat-label>
  <mat-select [(ngModel)]="selectedValue" [required]="required" [disabled]="disabled">
    <mat-option *ngFor="let option of options" [value]="option.value">{{ option.label }}</mat-option>
  </mat-select>
</mat-form-field>


usecase
<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <app-custom-select
    label="Select an option"
    [options]="selectOptions"
    [required]="true"
    formControlName="customSelect"
  ></app-custom-select>
  <button type="submit" [disabled]="form.invalid">Submit</button>
</form>




import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-custom-form',
  templateUrl: './custom-form.component.html',
  styleUrls: ['./custom-form.component.css']
})
export class CustomFormComponent implements OnInit {
  form: FormGroup;
  selectOptions = [
    { label: 'Option 1', value: 'option1' },
    { label: 'Option 2', value: 'option2' },
    { label: 'Option 3', value: 'option3' }
  ];

  constructor(private formBuilder: FormBuilder) { }

  ngOnInit(): void {
    this.form = this.formBuilder.group({
      customSelect: ['', Validators.required]
    });
  }

  onSubmit() {
    if (this.form.valid) {
      console.log('Form submitted:', this.form.value);
    }
  }
}



