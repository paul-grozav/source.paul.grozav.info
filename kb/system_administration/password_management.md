---
layout: page
ptitle: Password management
---

{% highlight sh %}
//----------------------------------------------------------------------------//
(
now="2020-04-07 17:54:15.560144483" &&
now="$(date +"%Y-%m-%d %H:%M:%S.%N")" &&
new_pwd="${now}" &&
# Order members alphabetically
new_pwd="$(echo "${new_pwd}" ; cat - <<EOF
Gary Winston
John Doe
Martha Joseph
EOF
)" &&
new_pwd="$(echo "${new_pwd}" | openssl dgst -sha256 | awk '{print $2}')" &&
echo "For now = ${now}" &&
echo "password = ${new_pwd}" &&
echo "short password = ${new_pwd:0:32}"
)
//----------------------------------------------------------------------------//
{% endhighlight %}

### 1. Advantages:
1.1. No one "knows" the password - you have to have it written down on
something.

1.2. Changing the team, means changing the password

1.3. Even if someone knows the algorithm, it must also know the exact time
and team structure, in order to obtain the password.

### 2. Disadvantages:
2.1. You have to have it written down, someone could steal it.
