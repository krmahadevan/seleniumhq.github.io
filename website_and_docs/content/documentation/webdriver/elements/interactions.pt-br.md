---
title: "Interacting with web elements"
linkTitle: "Interactions"
weight: 2
description: >
   A high-level instruction set for manipulating form controls.
needsTranslation: true
---

There are only 5 basic commands that can be executed on an element:
* [click](https://w3c.github.io/webdriver/#element-click) (applies to any element)
* [send keys](https://w3c.github.io/webdriver/#element-send-keys) (only applies to text fields and content editable elements)
* [clear](https://w3c.github.io/webdriver/#element-send-keys) (only applies to text fields and content editable elements)
* submit (only applies to form elements)
* select (see [Select List Elements](select_lists.md))

## Additional validations

These methods are designed to closely emulate a user's experience, so,
unlike the [Actions API]({{< ref "/documentation/webdriver/actions_api/" >}}), it attempts to perform two things
before attempting the specified action.
1. If it determines the element is outside the viewport, it
   [scrolls the element into view](https://w3c.github.io/webdriver/#dfn-scrolls-into-view), specifically
   it will align the bottom of the element with the bottom of the viewport.
2. It ensures the element is [interactable](https://w3c.github.io/webdriver/#interactability)
   before taking the action. This could mean that the scrolling was unsuccessful, or that the
   element is not otherwise displayed.  Determining if an element is displayed on a page was too difficult to
   [define directly in the webdriver specification](https://w3c.github.io/webdriver/#element-displayedness),
   so Selenium sends an execute command with a JavaScript atom that checks for things that would keep
   the element from being displayed. If it determines an element is not in the viewport, not displayed, not
   [keyboard-interactable](https://w3c.github.io/webdriver/#dfn-keyboard-interactable), or not
   [pointer-interactable](https://w3c.github.io/webdriver/#dfn-pointer-interactable),
   it returns an [element not interactable](https://w3c.github.io/webdriver/#dfn-element-not-interactable) error.

## Click

The [element click command](https://w3c.github.io/webdriver/#dfn-element-click) is executed on
the [center of the element](https://w3c.github.io/webdriver/#dfn-center-point).
If the center of the element is [obscured](https://w3c.github.io/webdriver/#dfn-obscuring) for some reason,
Selenium will return an [element click intercepted](https://w3c.github.io/webdriver/#dfn-element-click-intercepted) error.

## Send keys

The [element send keys command](https://w3c.github.io/webdriver/#dfn-element-send-keys)
types the provided keys into an [editable](https://w3c.github.io/webdriver/#dfn-editable) element.
Typically, this means an element is an input element of a form with a `text` type or an element
with a`content-editable` attribute. If it is not editable,
[an invalid element state](https://w3c.github.io/webdriver/#dfn-invalid-element-state) error is returned.

[Here](https://www.w3.org/TR/webdriver/#keyboard-actions) is the list of
possible keystrokes that WebDriver Supports.

{{< tabpane >}}
{{< tab header="Java" >}}
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

public class HelloSelenium {
public static void main(String[] args) {
WebDriver driver = new FirefoxDriver();
try {
// Navigate to Url
driver.get("https://google.com");

      // Enter text "q" and perform keyboard action "Enter"
      driver.findElement(By.name("q")).sendKeys("q" + Keys.ENTER);
    } finally {
      driver.quit();
    }
}
}

{{< /tab >}}
{{< tab header="Python" >}}
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
driver = webdriver.Firefox()

    # Navigate to url
driver.get("http://www.google.com")

    # Enter "webdriver" text and perform "ENTER" keyboard action
driver.find_element(By.NAME, "q").send_keys("webdriver" + Keys.ENTER)
{{< /tab >}}
{{< tab header="CSharp" >}}
using (var driver = new FirefoxDriver())
{
// Navigate to Url
driver.Navigate().GoToUrl("https://google.com");

// Enter "webdriver" text and perform "ENTER" keyboard action
driver.FindElement(By.Name("q")).SendKeys("webdriver" + Keys.Enter);
}
{{< /tab >}}
{{< tab header="Ruby" >}}
require 'selenium-webdriver'
driver = Selenium::WebDriver.for :firefox
begin
# Navigate to URL
driver.get 'https://google.com'

    # Enter "webdriver" text and perform "ENTER" keyboard action
driver.find_element(name: 'q').send_keys 'webdriver', :return

ensure
driver.quit
end
{{< /tab >}}
{{< tab header="JavaScript" >}}
const {Builder, By, Key} = require('selenium-webdriver');

(async function example() {
let driver = await new Builder().forBrowser('firefox').build();

try {
// Navigate to Url
await driver.get('https://www.google.com');

    // Enter text "webdriver" and perform keyboard action "Enter"
    await driver.findElement(By.name('q')).sendKeys('webdriver', Key.ENTER);
}
finally {
await driver.quit();
}
})();
{{< /tab >}}
{{< tab header="Kotlin" >}}
import org.openqa.selenium.By
import org.openqa.selenium.Keys
import org.openqa.selenium.firefox.FirefoxDriver

fun main() {
val driver = FirefoxDriver()
try {
// Navigate to Url
driver.get("https://google.com")

    // Enter text "q" and perform keyboard action "Enter"
    driver.findElement(By.name("q")).sendKeys("q" + Keys.ENTER)
} finally {
driver.quit()
}
}
{{< /tab >}}
{{< /tabpane >}}

## Clear

The [element clear command](https://w3c.github.io/webdriver/#dfn-element-clear) resets the content of an element.
This requires an element to be [editable](https://w3c.github.io/webdriver/#dfn-editable),
and [resettable](https://w3c.github.io/webdriver/#dfn-resettable-elements). Typically,
this means an element is an input element of a form with a `text` type or an element
with a`content-editable` attribute. If these conditions are not met,
[an invalid element state](https://w3c.github.io/webdriver/#dfn-invalid-element-state) error is returned.

{{< tabpane langEqualsHeader=true >}}
{{< tab header="Java" >}}
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class clear {
public static void main(String[] args) {
WebDriver driver = new ChromeDriver();
try {
// Navigate to Url
driver.get("https://www.google.com");
// Store 'SearchInput' element
WebElement searchInput = driver.findElement(By.name("q"));
searchInput.sendKeys("selenium");
// Clears the entered text
searchInput.clear();
} finally {
driver.quit();
}
}
}
{{< /tab >}}
{{< tab header="Python" >}}
from selenium import webdriver
from selenium.webdriver.common.by import By
driver = webdriver.Chrome()

    # Navigate to url
driver.get("http://www.google.com")
# Store 'SearchInput' element
SearchInput = driver.find_element(By.NAME, "q")
SearchInput.send_keys("selenium")
# Clears the entered text
SearchInput.clear()
{{< /tab >}}
{{< tab header="CSharp" >}}
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using System;

namespace SnipetProjectDelete
{
class Program
{
static void Main(string[] args)
{
IWebDriver driver = new ChromeDriver();
try
{
// Navigate to Url
driver.Navigate().GoToUrl(@"https://www.google.com");
// Store 'SearchInput' element
IWebElement searchInput = driver.FindElement(By.Name("q"));
searchInput.SendKeys("selenium");
// Clears the entered text
searchInput.Clear();
}
finally
{
driver.Quit();
}
}
}
}
{{< /tab >}}
{{< tab header="Ruby" >}}
require 'selenium-webdriver'
driver = Selenium::WebDriver.for :chrome
begin
# Navigate to URL
driver.get 'https://google.com'
# store 'search_input' element
search_input = driver.find_element(name: 'q')
search_input.send_keys('selenium')
# Clears the entered text
search_input.clear
ensure
driver.quit
end
{{< /tab >}}
{{< tab header="JavaScript" >}}
const {Builder, By} = require('selenium-webdriver');
(async function example() {
let driver = await new Builder().forBrowser('chrome').build();
try {
// Navigate to Url
await driver.get('https://www.google.com');
// Store 'SearchInput' element
let searchInput = driver.findElement(By.name('q'));
await searchInput.sendKeys("selenium");
// Clears the entered text
await searchInput.clear();
}
finally {
await driver.quit();
}
})();
{{< /tab >}}
{{< tab header="Kotlin" >}}
import org.openqa.selenium.By
import org.openqa.selenium.chrome.ChromeDriver
fun main() {
val driver =  ChromeDriver()
try {
// Navigate to Url
driver.get("https://www.google.com")
// Store 'searchInput' element
val searchInput = driver.findElement(By.name("q"))
searchInput.sendKeys("selenium")
// Clears the entered text
searchInput.clear()
} finally {
driver.quit()
}
}
{{< /tab >}}
{{< /tabpane >}}

## Submit

In Selenium 4 this is no longer implemented with a separate endpoint and functions by executing a script. As
such, it is recommended not to use this method and to click the applicable form submission button instead.

