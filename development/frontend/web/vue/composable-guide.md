## Overview

The `useRegister` composable encapsulates all the logic needed for a user registration form, including form **state management**, **validation**, and **submission handling**.

## Key Components
### Form Data Management

```javascript
const formData = reactive({
  firstName: '',
  lastName: '',
  email: '',
  password: '',
  confirmPassword: ''
});
```

- Uses Vue's reactive to create a reactive object for form fields
- Tracks all user input fields required for registration

### Error Handling

```javascript
const errors = reactive({
  firstName: '',
  lastName: '',
  email: '',
  password: '',
  confirmPassword: ''
});
```

- Reactive object to store field-specific validation errors
- Each field has a corresponding error message property

### State Management

- `isLoading`: Tracks submission state (shows loading indicators)
- `submitError`: Stores server-side or general submission errors
- `submitSuccess`: Boolean flag indicating successful registration

### Validation Rules

The composable defines comprehensive validation rules for each field:

- **Names**: Required, 2-50 characters
- **Email**: Required, valid email format
- **Password**: Required, minimum 8 characters, must contain uppercase, lowercase, number, and special character
- **Confirm Password**: Must match the password field

### Key Functions

1. `validateField(field)`: Validates a single field against its rules
2. `clearErrors()`: Resets all error states
3. `handleSubmit()`: Main submission handler that:
    + Clears previous errors
    + Validates the entire form
    + Calls the authentication service
    + Handles success/error responses
    + Resets form on success

### Computed Properties

`isFormValid`: Automatically determines if the form is valid based on all fields being filled and having no errors


### Usage Pattern

This composable follows Vue 3's composition pattern, returning all reactive state and functions that a component needs to implement a registration form. A component would destructure the returned object and bind the reactive properties to form inputs and the functions to form events.

The composable integrates with your authentication service and validation utilities, providing a clean separation of concerns between UI components and business logic.

## How the Integration Works

### 1. Composable Import & Destructuring

```javascript
const {
  formData,
  errors,
  isLoading,
  submitError,
  submitSuccess,
  isFormValid,
  validateField,
  handleSubmit
} = useRegister();
```

The `RegisterView` imports and destructures all the reactive state and functions from `useRegister`, making them available in the component's template and script.

### 2. Form Data Binding

Each input field is bound to the composable's reactive `formData`:

```html
<input v-model="formData.firstName">
<input v-model="formData.email">
<!-- etc. -->
```

This creates two-way data binding - when users type, the composable's state updates automatically.

### 3. Real-time Validation

The component triggers validation on blur events:

```html
<input @blur="validateField('firstName')">
```

There's also a clever bit where changing the password field triggers confirm password validation:

```html
<input @input="validateField('confirmPassword')">
```

This ensures the confirm password field re-validates when the main password changes.

### 4. Error Display

Each field shows its validation error from the composable:

```html
<input :class="{ 'is-danger': errors.firstName }">
<p v-if="errors.firstName" class="help is-danger">{{ errors.firstName }}</p>
```

The Bulma CSS framework styles are applied conditionally based on error state.

### 5. Form Submission

```html
<form @submit.prevent="handleSubmit">
```

The form prevents default submission and calls the composable's `handleSubmit` function, which handles all the validation and API calls.

### 6. UI State Management

The component responds to various states from the composable:

- **Loading State**: Button shows loading spinner and disables form
    ```html
    <button :class="{ 'is-loading': isLoading }" :disabled="isLoading || !isFormValid">
    ```
- **Success State**: Shows green notification
    ```html
    <div v-if="submitSuccess" class="notification is-success">  
    ```
- **Error State**: Shows red notification
    ```html
    <div v-if="submitError" class="notification is-danger">
    ```

### 7. Form Validation State

The submit button is only enabled when the form is valid:

```html
:disabled="isLoading || !isFormValid"
```

The `isFormValid` computed property from the composable automatically determines this.

### Key Benefits of This Pattern

1. **Separation of Concerns**: The view handles presentation, the composable handles logic
2. **Reusability**: The `useRegister` composable could be used in other components
3. **Testability**: Business logic in the composable can be tested independently
4. **Reactivity**: All state changes automatically update the UI
5. **Type Safety**: Clear interface between component and composable

#### Smart UX Features
- **Progressive Validation**: Fields validate on blur, not on every keystroke
- **Password Confirmation**: Automatically re-validates when password changes
- **Form State Management**: Submit button disabled until form is valid
- **Loading States**: Prevents double submission and shows user feedback
- **Success/Error Feedback**: Clear user communication