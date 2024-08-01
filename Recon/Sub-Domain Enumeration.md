
After finding as many domains on the target as possible, locate as many subdomains on those domains as you can.


Tools like `Sublist3r`, `SubBrute`, `Amass`, and `Gobuster` can enumerate subdomains automatically with a variety of wordlists and strategies


For example, 

`Sublist3r` works by querying search engines and online subdomain databases,

`SubBrute` is a brute-forcing tool that guesses possible subdomains until it finds real ones.

`Amass` uses a combination of DNS zone transfers, certificate parsing, search engines, and subdomain databases to find subdomains

we can also make wordlists for ourselves:

Here’s a simple command to remove duplicate items from a set of two wordlists:

#CreatingWordList
```shell
sort -u wordlist1.txt wordlist2.txt
```

The `sort` command line tool sorts the lines of text files.

When given multiple files, it will sort all files and write the output to the terminal. 

The`-u` option tells `sort` to return only unique items in the sorted list.

`Gobuster` is a tool for brute-forcing to discover subdomains, directories, and files on target web servers. Its `DNS` mode is used for subdomain brute-forcing. 

In this mode, you can use the flag `-d` to specify the domain you
want to brute-force and `-w` to specify the wordlist you want to use:
#GobusterSubDomainEnum
```shell
gobuster dns -d target_domain -w wordlist
```


Once you’ve found a good number of subdomains, you can discover more by identifying patterns. For example, if you find two subdomains of `example.com` named `1.example.com` and `3.example.com`, you can guess that `2.example.com`is probably also a valid subdomain. 

A good tool for automating this process is `Altdns`
link:`https://github.com/infosec-au/altdns/`,
which discovers subdomains with names that are 
permutations of other subdomain names.

In addition, you can find more subdomains based on your knowledge about the company’s technology stack. For example, if you’ve already learned that `example.com` uses `Jenkins`, you can check if `jenkins.example.com` is a valid subdomain.

 After you’ve found sub domains , like, `dev.example.com`, you might find subdomains like `1.dev.example.com`. You can find subdomains of subdomains by running enumeration tools recursively: add the results of your first run to your Known Domains list and run the tool again