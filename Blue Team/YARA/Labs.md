Identifying malware is integral to understanding threats and avoiding infection; Yara is capable of scanning malicious files, designed specifically for this purpose. This series will expand on how to use Yara effectively and create rules to analyze malware samples and identify threats.

Hint
String sets
When creating the condition for the Yara rule there is some standard syntax we can use to match groups.

rule ExampleRule
{
strings:
$a = "bar"
$a2 = "foo"
$a3 = "cow"
$b = "hello"
$b2 = "enterprise"
$c = "world"

condition:
1 of them // equivalent to 1 of ($*)

}

- all of them | this will match files that have all of the strings
- any of them | this will match files that have at least one of the strings
- all of ($a*) | this will match files that have all of the strings in $a, $a2 and $a3
- any of ($b*) | this will match files that have any of the strings in $b and $b2

