# AdminLTE 3 widgets for Yii2

[![Latest Version](https://img.shields.io/github/release/co0lc0der/yii2-adminlte3-widgets?style=flat-square)](https://github.com/co0lc0der/yii2-adminlte3-widgets/release)
[![Packagist Downloads](https://img.shields.io/packagist/dt/co0lc0der/yii2-adminlte3-widgets?color=yellow&style=flat-square)](https://packagist.org/packages/co0lc0der/yii2-adminlte3-widgets)
[![GitHub license](https://img.shields.io/github/license/co0lc0der/yii2-adminlte3-widgets?style=flat-square)](https://github.com/co0lc0der/yii2-adminlte3-widgets/blob/main/LICENSE.md)

AdminLTE 3 widgets for Yii2. At present time the extension includes

* [CardToolsSupportTrait](#cardtoolssupporttrait)
* [ShadowSupportTrait](#shadowsupporttrait)
* [CardToolsHelper](#cardtoolshelper)
* [CardWidget](#cardwidget)
* [TabsCardWidget](#tabscardwidget)
* [ProfileCardWidget](#profilecardwidget)
* [ContactCardWidget](#contactcardwidget)
* [DirectChatWidget](#directchatwidget)

**Based on [AdminLTE 3.1.0](https://github.com/ColorlibHQ/AdminLTE/releases/tag/v3.1.0)**. More widgets, helpers and Gii will be added in the future.

## Installation

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```sh
composer require --prefer-dist co0lc0der/yii2-adminlte3-widgets "*"
```

or add

```js
"co0lc0der/yii2-adminlte3-widgets": "*"
```

to the require section of your `composer.json` file.

## CardToolsSupportTrait

Adds support tool buttons (standard and custom tools) for a card header.

### Public properties, its types and default values

- `bool $collapse = true` - show / hide collapse button inside card header
- `bool $hide = false` - show / hide a collapsed card after initialization
- `bool $expand = false` - show / hide maximize button inside card header
- `bool $close = false` - show / hide close button inside card header
- `array $tools = []` - list of header tools (standard and custom labels, buttons, links)

## ShadowSupportTrait

Adds support a shadow for a card. You should call `getShadowClass()` in your overwritten `getCardBodyClass()` method if you use this trait.

### Public properties, its types and default values

- `string $shadow = ''` - type of card shadow ('shadow-none', 'shadow-sm', 'shadow', 'shadow-lg')

## CardToolsHelper

It helps to make buttons for a card header. See the example for CardWidget below.

## CardWidget

This is the basic class. It uses [CardToolsSupport](#cardtoolssupporttrait) and [ShadowSupport](#shadowsupporttrait) traits.

### Public properties, its types and default values

- `string $title = ''` - title of a card
- `string $color = ''` - color of a card header (Bootstrap 4 colors. 'success', 'danger' еtс.)
- `bool $outline = false` - makes an outlined card
- `bool $background = false` - makes a colored card, uses $color property (Bootstrap 4 colors)
- `bool $gradient = false` - makes a gradient card, uses $color property (Bootstrap 4 colors)
- `array|string $footer = ''` - content of card footer
- `string $ajaxLoad = ''` - URL for loading data, if it is not empty it shows a spinner before data loaded
- `string $ajaxOverlay = 'overlay'` - type of loading overlay ('overlay', 'dark')
- `array $cssClasses = []` - additional CSS classes, these classes will be added to the basic class. format:

```php
[
    0 => 'classes-for-card-wrapper',
    1 => 'classes-for-card-header',
    2 => 'classes-for-card-title',
    3 => 'classes-for-card-body',
    4 => 'classes-for-card-footer',
]
```

### Example 1

```php
<?php CardWidget::begin([
    'title' => 'Some title',    // title of a card
    'color' => 'dark',          // bootstrap color name 'success', 'danger' еtс.
    'gradient' => true,         // use gradient background
    'expand' => true,           // show maximize button in card header
    'footer' => 'some footer',  // content of card footer
    'shadow' => 'shadow-sm',    // use small shadow
    'close' => true,            // show close button in card header
    'tools' => [                // array with config to add custom labels, buttons or links
        CardToolsHelper::label('new', [
            'class' => 'badge badge-primary',
            'title' => 'New',
        ]),
        // OR you can use classic array
        /*[
            'label',
            'new',
            [
                'class' => 'badge badge-primary',
                'title' => 'New',
            ],
        ],*/
        CardToolsHelper::a(['update', 'id' => 1], 'pencil-alt', '', ['title' => 'Update it']),
        // OR you can use classic array
        /*[
            'link',
            '<i class="fas fa-pencil-alt"></i>',
            ['update', 'id' => 1],
            ['title' => 'Update it'],
        ],*/
        CardToolsHelper::button('cog', '', ['title' => 'some tooltip']),
        // OR you can use classic array
        /*[
            'button',
            '<i class="fas fa-cog"></i>',
            ['title' => 'some tooltip'],
        ],*/
    ],
]); ?>

<?= 'some content'; ?>

<?php CardWidget::end(); ?>
```

### Rendered card

![Rendered card](https://code-notes.ru/card_example.png "Rendered card")

### Example 2

```php
<?php CardWidget::begin([
    'title' => 'Folders',
    'cssClasses' => [3 => 'p-0'],
]); ?>

<ul class="nav nav-pills flex-column">
    <li class="nav-item active">
        <a href="#" class="nav-link">
            <i class="fas fa-inbox"></i> Inbox
            <span class="badge bg-primary float-right">12</span>
        </a>
    </li>
    <li class="nav-item">
        <a href="#" class="nav-link">
            <i class="far fa-envelope"></i> Sent
        </a>
    </li>
    <li class="nav-item">
        <a href="#" class="nav-link">
            <i class="far fa-file-alt"></i> Drafts
            <span class="badge bg-warning float-right">3</span>
        </a>
    </li>
    <li class="nav-item">
        <a href="#" class="nav-link">
            <i class="far fa-trash-alt"></i> Trash
        </a>
    </li>
</ul>

<?php CardWidget::end(); ?>
```

### Rendered card

![Rendered card](https://code-notes.ru/card_example2.png "Rendered card")

### Header dropdown button and footer button example

Support since v0.5.2. Use `['---']` to put a divider into header dropdown menu. Array format:

```php
['URL', 'link content', ['options']],
['---'],
['#', 'item 3', ['title' => 'THIS IS ITEM #3!']],
```

```php
<?php CardWidget::begin([
    'title' => 'Create Actions',
    'color' => 'lime',
    'outline' => true,
    'collapse' => false,
    'cssClasses' => [4 => 'p-1'],
    'footer' => [
        [
            '<i class="fas fa-comments"></i>',
            'bg-secondary',
            ['update', 'id' => 1],
        ],
    ],
    'tools' => [
        CardToolsHelper::submenu([
            ['#1', 'item 1'],
            ['#2', 'item 2'],
            ['---'],
            ['#3', 'item 3', ['title' => 'THIS IS ITEM #3!']],
        ], 'bars', true),
        // OR you can use classic array
        /*[
            [
                ['#1', 'item 1'],
                ['#2', 'item 2'],
                ['---'],
                ['#3', 'item 3', ['title' => 'THIS IS ITEM #3!']],
            ],
            'bars',
            true,
        ],*/
    ],
]); ?>
    <p>
        Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
        Aenean commodo ligula eget dolor. Aenean massa.
    </p>
<?php CardWidget::end(); ?>
```

### Rendered card

![Rendered card](https://code-notes.ru/card_example3.png "Rendered card")

## TabsCardWidget

It uses [ShadowSupport trait](#shadowsupporttrait).

### Public properties, its types and default values

- `string $title = ''` - title of a card, if title is empty tabs will be placed on the left side of the card header
- `string $color = ''` - color of a card header (Bootstrap 4 colors. 'success', 'danger' еtс.)
- `bool $outline = false` - makes an outlined card
- `bool $background = false` - makes a colored card, uses $color property (Bootstrap 4 colors)
- `bool $gradient = false` - makes a gradient card, uses $color property (Bootstrap 4 colors)
- `string $footer = ''` - content of card footer
- `array $tabs = []` - list of tabs (see an example below)

### Example

```php
<?= TabsCardWidget::widget([
    'title' => 'Tabs example',
    'footer' => 'some footer',
    'tabs' => [
        [
            'title' => 'Tab1',
            'id' => 'tab_1',
            'content' => 'A wonderful serenity has taken possession of my entire soul,
                like these sweet mornings of spring which I enjoy with my whole heart.',
            'active' => true,
        ],
        [
            'title' => 'Tab2',
            'id' => 'tab_2',
            'content' => 'The European languages are members of the same family. Their separate existence is a myth.
                For science, music, sport, etc, Europe uses the same vocabulary.',
        ]
    ]
]); ?>
```

### Rendered TabsCard

![Rendered TabsCard](https://code-notes.ru/tabscard_example.png "Rendered TabsCard")

### Rendered TabsCard without title

![Rendered TabsCard without title](https://code-notes.ru/tabscard_example2.png "Rendered TabsCard without title")

## ProfileCardWidget

This class is extended of CardWidget therefore it has the same properties but it has its own properties listed below.

### Public properties (excluding CardWidget properties), its types and default values

- `string $name` - user name
- `string $image = ''` - user image
- `string $position = ''` - user role or position
- `array $rows = []` - list of rows (see an example below)

### Example

```php
<?php ProfileCardWidget::begin([
    'name' => 'Jonathan Burke Jr.',
    'position' => 'Software Engineer',
    'image' => '../avatars/user2-160x160.jpg',
    'color' => 'info',
    'outline' => true,
    'rows' => [
        'Followers' => [
            '1,521',
            '#url'
        ],
        'Following'	=> ['373'],
        'Friends'	=> ['3,127'],
        'Projects'	=> [
            '7',
            'https://example.com'
        ],
    ],
]); ?>

<a href="#" class="btn btn-primary btn-block"><b>Follow</b></a>

<?php ProfileCardWidget::end();?>
```

### Rendered ProfileCard

![Rendered ProfileCard](https://code-notes.ru/profilecard_example.png "Rendered ProfileCard")

## ContactCardWidget

You should use `echo` and `widget()` method. It uses [CardToolsSupport](#cardtoolssupporttrait) and [ShadowSupport](#shadowsupporttrait) traits.

### Public properties, its types and default values

- `string $name` - user name
- `string $image = ''` - user image
- `string $position = ''` - user role or position (title of a card)
- `array|string $about = ''` - about user. format: array ['Web Designer', 'UX'] or string
- `string $aboutTitle = 'About: '` - about title
- `string $aboutSeparator = ' / '` - separator of about user if it is an array
- `array $info = []` - list of rows. format: FontAwesome icon => text
- `string $color = ''` - color of a card header (Bootstrap 4 colors. 'success', 'danger' еtс.)
- `bool $outline = false` - makes an outlined card
- `string|array $footer = ''` - content of card footer, it can be some string or an array of buttons

### Example

```php
<?= ContactCardWidget::widget([
    'name' => 'Jonathan Burke Jr.',
    'position' => 'Software Engineer',
    'image' => '../avatars/user2-160x160.jpg',
    'color' => 'info',
    'outline' => true,
    'close' => true,
    'aboutTitle' => 'Skills: ',
    'about' => ['Web Designer', 'UX', 'Graphic Artist', 'Coffee Lover'],
    'info' => [
        'fa-building' => 'Address: Demo Street 123, Demo City 04312, NJ',
        'fa-phone' => 'Phone #: <a href="tel:+80012122352">+ 800 - 12 12 23 52</a>',
        'fa-envelope' => 'Email: <a href="mailto:jonatan@example.com">jonatan@example.com</a>',
    ],
    'footer' => [
        [
            '<i class="fas fa-comments"></i>',
            'bg-teal',
            ['update', 'id' => 1],
            [],
        ],
        [
            '<i class="fas fa-user"></i> View profile',
            'btn-primary',
            '#profile',
            [],
        ],
    ],
]); ?>
```

### Rendered ContactCard

![Rendered ContactCard](https://code-notes.ru/contactcard_example.png "Rendered ContactCard")

## DirectChatWidget

This class is extended of CardWidget therefore it has the same properties but it has its own properties listed below.

### Public properties (excluding CardWidget properties), its types and default values

- `string $chatColor = 'primary'` - chat messages color (Bootstrap 4 colors. 'success', 'danger' еtс.)
- `string $author = ''` - author's name, this property is used to highlight author's messages with $chatColor
- `array $messages = []` - array of messages
- `array $contacts = []` - contacts list. if it is empty there will be no chat icon in the header of the chat
- `string $noMessages = 'There is no messages in the chat'` - the message which will be shown if $messages is empty
- `string $sendFormUrl = ''` - URL to send a new message
- `string $sendFormButtonTitle = 'Send'` - title of send button
- `string $sendFormPlaceholder = 'Type Message ...'` - placeholder for send form

### Example

```php
<?php DirectChatWidget::begin([
    'title' => 'Admin direct chat',
    'color' => 'info',
    'chatColor' => 'info',
    'close' => true,
    'author' => 'Admin',
    'sendFormPlaceholder' => 'Type your message here ...',
    'sendFormButtonTitle' => '<i class="fas fa-paper-plane"></i>',
    'tools' => [
        CardToolsHelper::label('3', [
            'class' => 'badge badge-dark',
            'title' => '3 new messages',
        ]),
        // OR you can use classic array
        /*[
            'label',
            '3',
            [
                'class' => 'badge badge-dark',
                'title' => '3 new messages',
            ],
        ],*/
    ],
    'messages' => [
        [
            'Admin',
            '23 Jan 2:00 pm',
            '../assets/img/user1-128x128.jpg',
            'Is this template really for free? That\'s unbelievable!',
        ],
        [
            'Sarah Bullock',
            '23 Jan 2:04 pm',
            '../assets/img/user5-128x128.jpg',
            'You better believe it!',
        ],
        [
            'Admin',
            '23 Jan 5:07 pm',
            '../assets/img/user1-128x128.jpg',
            'Working with AdminLTE on a great new app! Wanna join?',
        ],
        [
            'Sarah Bullock',
            '23 Jan 6:10 pm',
            '../assets/img/user5-128x128.jpg',
            'I would love to.',
        ],
        [
            'Admin',
            '25 Jan 1:00 pm',
            '../assets/img/user1-128x128.jpg',
            'test message!',
        ],
    ],
    'contacts' => [
        [
            'Admin',
            '1/28/2022',
            '../assets/img/user1-128x128.jpg',
            'How have you been? I was...',
            '#',
        ],
        [
            'Sarah Bullock',
            '1/28/2022',
            '../assets/img/user5-128x128.jpg',
            'I will be waiting for...',
            '#link_to_profile',
        ],
    ],
]); ?>

<!-- you can manually put HTML messages here* -->
<!-- you can manually put HTML contacts here* -->

<?php DirectChatWidget::end(); ?>
```

\* - leave empty $messages and/or $contacts properties and see [AdminLTE documentation](https://adminlte.io/docs/3.1/components/direct-chat.html) for examples.

### Rendered DirectChat

![Rendered DirectChat](https://code-notes.ru/directchat_example1.png "Rendered DirectChat")

### Rendered DirectChat (Contact list)

![Rendered DirectChat (Contact list)](https://code-notes.ru/directchat_example2.png "Rendered DirectChat (Contact list)")
