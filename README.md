# Email Field Type

*anomaly.field_type.email*

#### An email input field type.

The email field type provides an HTML5 email input with built-in email validation.

## Features

- HTML5 email input with browser validation
- Automatic email format validation
- Support for placeholder text
- Custom input classes
- Built-in RFC 5322 email validation
- Database storage optimization
- Integrates with Laravel validation

## Configuration

### Basic Configuration

```php
protected $fields = [
    'email' => [
        'type' => 'anomaly.field_type.email'
    ]
];
```

### With Placeholder

```php
'email' => [
    'type'   => 'anomaly.field_type.email',
    'config' => [
        'placeholder' => 'user@example.com'
    ]
]
```

### With Custom Class

```php
'email' => [
    'type'   => 'anomaly.field_type.email',
    'config' => [
        'class' => 'form-control-lg'
    ]
]
```

## Usage Examples

### Basic Email Field

```php
$stream->create([
    'email' => 'user@example.com'
]);
```

### With Additional Validation

```php
protected $fields = [
    'contact_email' => [
        'type'  => 'anomaly.field_type.email',
        'rules' => [
            'required',
            'email',
            'unique:users,email'
        ]
    ]
];
```

## Accessing Values

### In Twig Templates

```twig
{# Display email #}
{{ entry.email }}

{# Create mailto link #}
<a href="mailto:{{ entry.email }}">{{ entry.email }}</a>

{# Check if email exists #}
{% if entry.email %}
    <p>Contact: {{ entry.email }}</p>
{% endif %}
```

### In PHP

```php
$entry = $model->find(1);

// Get email value
$email = $entry->email;

// Send email
Mail::to($entry->email)->send(new WelcomeEmail());
```

## Setting Values

### In Forms

```php
$form = $builder->make('example.module.test');
$form->on('saving', function(FormBuilder $builder) {
    $entry = $builder->getFormEntry();
    $entry->email = 'newemail@example.com';
});
```

### Direct Assignment

```php
$entry->email = 'contact@example.com';
$entry->save();
```

## Database Structure

The email field type stores email addresses as:
- **VARCHAR(255)** - The email address

## Validation

### Required Email

```php
'email' => [
    'type'  => 'anomaly.field_type.email',
    'rules' => [
        'required',
        'email'
    ]
]
```

### Unique Email

```php
'email' => [
    'type'  => 'anomaly.field_type.email',
    'rules' => [
        'required',
        'email',
        'unique:users,email'
    ]
]
```

### Email with Max Length

```php
'email' => [
    'type'  => 'anomaly.field_type.email',
    'rules' => [
        'required',
        'email',
        'max:255'
    ]
]
```

### Multiple Emails (Comma Separated)

```php
'emails' => [
    'type'  => 'anomaly.field_type.email',
    'rules' => [
        'required',
        function ($attribute, $value, $fail) {
            $emails = explode(',', $value);
            foreach ($emails as $email) {
                if (!filter_var(trim($email), FILTER_VALIDATE_EMAIL)) {
                    $fail("The $attribute must contain valid email addresses.");
                }
            }
        }
    ]
]
```

## Common Use Cases

### User Registration Email

```php
'email' => [
    'type'  => 'anomaly.field_type.email',
    'rules' => [
        'required',
        'email',
        'unique:users,email'
    ]
]
```

### Contact Form Email

```php
'contact_email' => [
    'type'   => 'anomaly.field_type.email',
    'config' => [
        'placeholder' => 'your@email.com'
    ],
    'rules' => [
        'required',
        'email'
    ]
]
```

### Reply-To Email

```php
'reply_to' => [
    'type'   => 'anomaly.field_type.email',
    'config' => [
        'placeholder' => 'reply@domain.com'
    ],
    'rules' => [
        'email'
    ]
]
```

### Notification Email List

```php
'notification_email' => [
    'type'   => 'anomaly.field_type.email',
    'config' => [
        'placeholder' => 'notifications@example.com'
    ],
    'rules' => [
        'required',
        'email'
    ]
]
```

## Best Practices

1. **Always Validate**: Use the `email` validation rule for proper format checking
2. **Consider Uniqueness**: For user accounts, ensure email uniqueness with `unique` rule
3. **Use Placeholders**: Provide example formats to guide users
4. **Lowercase Storage**: Consider normalizing emails to lowercase before storage
5. **Verification**: Implement email verification for critical operations
6. **Privacy**: Handle email addresses according to privacy regulations (GDPR, etc.)
7. **Spam Protection**: Implement rate limiting for email-related operations

## Requirements

- Streams Platform ^1.10
- PyroCMS 3.10+

## License

The Email Field Type is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).

## Authors

PyroCMS, Inc. - [https://pyrocms.com](https://pyrocms.com)
Ryan Thompson - [support@pyrocms.com](mailto:support@pyrocms.com)
