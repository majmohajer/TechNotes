[HOME](home.md)  

### --strictPropertyInitialization
TypeScript's --strictPropertyInitialization compiler option ensures that a class initializes its properties during construction. When enabled, this option causes the TypeScript compiler to report an error if the class does not set a value to any property that is not explicitly marked as optional.

> By design, Angular treats all @Input properties as optional. When possible, you should satisfy --strictPropertyInitialization by providing a default value.

<pre>
@Component({
  selector: 'toh-hero',
  template: `...`,
})
export class HeroComponent {
  @Input() id = 'default_id';
}
</pre>

> If the property is hard to construct a default value for, use ? to explicitly mark the property as optional.

<pre>
@Component({
  selector: 'toh-hero',
  template: `...`,
})
export class HeroComponent {
  @Input() id?: string;
  process() {
    if (this.id) {
      // ...
    }
  }
}
</pre>

> You may want to have a required @Input field, meaning all your component users are required to pass that attribute. In such cases, use a default value. Just suppressing the TypeScript error with ! is insufficient and should be avoided because it will prevent the type checker from ensuring the input value is provided.> 

<pre>
@Component({
  selector: 'toh-hero',
  template: `...`,
})
export class HeroComponent {
  // The exclamation mark suppresses errors that a property is
  // not initialized.
  // Ignoring this enforcement can prevent the type checker
  // from finding potential issues.
  @Input() id!: string;
}
</pre>

### Duck Typing aka Structural Type System

One of TypeScript’s core principles is that type checking focuses on the shape that values have. This is sometimes called “duck typing” or “structural typing”.

In a structural type system, if two objects have the same shape, they are considered to be of the same type.

The shape-matching only requires a subset of the object’s fields to match.

### Narrowing   
<pre>
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}
</pre>

### What are Unions? 
