# React get a components prop types

When using TS with React you may want to get the types of another component.

```tsx
const MyButton: React.FC<{
    value: string;
    onChange: (newValue: string) => void;
}> = () => {
    return <button value={value} onchange={onChange} />;
};
type ButtonProps = React.ComponentProps<typeof MyButton>;

// equivalent:

type ButtonProps = { value: string; onChange: (newValue: string) => void };
```

 This can be particularly useful when using an external library that does not expose the types for a component.