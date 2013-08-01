#!/bin/bash

# parse options
usage (){
    echo 'usage:' `basename $0` '[ -a <author> ] [ -e <email> ] range file'
    exit
}

if [[ ! -d .git ]]; then
    echo "error: Not a git repository."
    exit
fi

while getopts "ha:e:" flag; do
  case $flag in
    a) author=$OPTARG;;
    e) email=$OPTARG;;
    h) usage;;
    \?) usage;;
  esac
done

shift $(($OPTIND - 1))

range=$1
file=$2

if [[ -z $file ]]; then
    echo "error: No file specified."
    exit
fi

# parse range pattern
range_arr=$(echo $range|tr ",-" "\n,")

# append *one* space to target lines
for r in $range_arr; do
    sed -i '' -e "$r s/$/ /" $file
done

# get real user and email
real_user=`git config user.name`
real_addr=`git config user.email`

# set fake user as current user
fake_user=${author:-$real_user}
fake_addr=${email:-$real_addr}
git config user.name $fake_user
git config user.email $fake_addr

# do commit
git add $file
git commit -m 'make some minor changes :P'

# restor real user and email address
git config user.name $real_user
git config user.email $real_addr

echo "Done yean!"
