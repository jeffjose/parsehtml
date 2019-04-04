#!/usr/bin/env python3

import sys
import argparse
from bs4 import BeautifulSoup
import requests
import fileinput
import simplejson as json

from  pygments import highlight
from pygments.lexers import PythonLexer
from pygments.formatters import TerminalTrueColorFormatter, Terminal256Formatter

parser = argparse.ArgumentParser(description = "Parse and filter HTML")

parser.add_argument('input', help = "Input URL. Use - for stdin", default = "-")
parser.add_argument('selector', help = "CSS Selector")
parser.add_argument('--indent', help = "Indent", default = 2, type = int)

args = parser.parse_args()

def parseHTML(input):

    if input.startswith('http'):
        html = requests.get(input).text

    elif input is "-":
        html = sys.stdin.read()

    return html

def formatter(str):
    try:
        j = json.loads(str)
    except:
        return str
    else:
        return highlight(json.dumps(j, indent = args.indent), PythonLexer(), Terminal256Formatter(style = "autumn"))

def printer(tags):

    for tag in tags:
        print(tag.prettify(formatter = formatter))

html = parseHTML(args.input)

soup = BeautifulSoup(html, features = 'html.parser')

matches = soup.select(args.selector)

printer(matches)
