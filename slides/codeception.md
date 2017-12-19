## Codeception

* Full stack testing framework
* Поддерживает популярные PHP фреймворки
* Тестирование UI через WebDriver
* Поддержка Gherkin (с версии 2.2)

---

## Обычный тест

```php
$I->amGoingTo('Navigate to Users page in /administrator/');
$I->amOnPage('administrator/users');
$I->expectTo('see Users page');
$I->clickToolbarButton('New');
$I->waitForText('Users: New', '30', ['css' => 'h1']);
$I->fillField('#name', $this->name);
$I->fillField('#username', $this->username);
$I->fillField('#email', $this->email);
$I->fillField('#password', $this->password);
$I->fillField('#password2', $this->password);
$I->clickToolbarButton('Save & Close');
$I->waitForText('Users', '30', ['css' => 'h1']);
$I->see('User successfully saved', '#message-container');
```

---

## Почему BDD с Codeception

* встроенные инструменты:
  * Selenium тестирование
  * тестирование БД
  * тестирование фреймворков
  * тестирование очередей
* множество форматов (Unit, Gherkin, Cest)

---

# А можно пример?

![](resources/whowe.jpg)

---

### Только может в этот раз как-то без плюмбусов?

![](resources/whowe.jpg)

---

### Ладно, уговорили... А сайтец на Джумле поднять сможете?

![](resources/client.jpg)

---

### Легко, как два пальца в розетку!

![](resources/whowe.jpg)

---

### Но только по BDD ;)

![](resources/client.jpg)

---

```gherkin
Feature: content
  In order to manage content article in the web
  As an owner
  I need to create modify trash publish and Unpublish content article

  Background:
    When I Login into Joomla administrator with username "admin" and password "admin"
    And I see the administrator dashboard

  Scenario: Create an Article
    Given There is a add content link
    When I create new content with field title as "Article One" and content as a "This is my first article"
    And I save an article
    Then I should see the article "Article One" is created    
```

[Больше сценариев >](https://github.com/joomla-projects/gsoc16_browser-automated-tests/blob/staging/tests/codeception/acceptance4/administratorlogin.feature)

---

```php
/**
 * @When I Login into Joomla administrator with username :arg1 and password :arg2
 */
public function loginIntoJoomlaAdministrator($username, $password)
{
  $I = $this;
  $I->amOnPage(ControlPanelPage::$url);
  $I->fillField(LoginPage::$usernameField, $username);
  $I->fillField(LoginPage::$passwordField, $password);
  $I->click(LoginPage::$loginButton);
}
/**
 * @Then I should see the administrator dashboard
 * @When I see the administrator dashboard
 */
public function iShouldSeeTheAdministratorDashboard()
{
  $I = $this;
  $I->adminPage->waitForPageTitle(ControlPanelPage::$pageTitle);
  $I->see(ControlPanelPage::$pageTitle, AdminPage::$pageTitle);
}
/**
 * @Given There is a add content link
 */
public function thereIsAAddContentLink()
{
  $I = $this;
  $I->amOnPage(ArticleManagerPage::$url);
  $I->adminPage->clickToolbarButton('New');
}  

```

---

## А как это может работать в Ecommerce?

![](resources/client.jpg)

---

```gherkin
Feature: Adding a simple product to the cart
    In order to select products for purchase
    As a Visitor
    I want to be able to add simple products to cart

    Scenario: Adding a simple product to the cart
        Given the store has a product "T-shirt banana" priced at "$12.54"
        When I add this product to the cart
        Then I should be on my cart summary page
        And I should be notified that the product has been successfully added
        And there should be one item in my cart
        And this item should have name "T-shirt banana"
```

[пример взят из Sylius](https://github.com/Sylius/Sylius/blob/master/features)

---

## Как работать?

* размещаем спецификации в features
* делаем symlink с features ⇒ tests/{тип теста}
* пишем сценарные шаги для каждого вида тестов

---

## Типы тестов

* Functional - без UI, но с фреймворком
```
  ln -s $PWD/features ⇒ tests/functional
```
* Unit - компоненты системы
```
  ln -s $PWD/features ⇒ tests/unit
```

* Acceptance - UI + WebServer 
```
  ln -s $PWD/features ⇒ tests/acceptance
```

---

## Тест != Feature

* Не все тесты - часть спецификации
* Баги не являются спецификацией
* Описание всех возможных сценариев в Gherkin добавляет информационный шум

---

## Features ∈ Tests

* Feature описывают предполагаемые сценарии
* Regression test описывает проблемные сценарии
* Regression test `@depends` on Feature

---


## Работаем!

* Пишем спецификации!
* Пишем тесты!
* Делаем плюмбусы!

![](resources/happy.jpg)

---

## Спасибо!

Михаил Боднарчук @davert

**[Thinking-patterns.space](http://thinking-patterns.space)**

---
* Codeception BDD Guide [codeception.com/docs/07-BDD](http://codeception.com/docs/07-BDD)
* Plumbus-PHP [github/remotelyliving/plumbus-php](https://github.com/remotelyliving/plumbus-php)
* Joomla BDD Examples [github/joomla-projects/gsoc16_browser-automated-tests](https://github.com/joomla-projects/gsoc16_browser-automated-tests/tree/staging/tests/codeception)
* Liz Keogh: BDD [lizkeogh.com/behaviour-driven-development/](https://lizkeogh.com/behaviour-driven-development/)
* Dan North: What's in a Story [dannorth.net/whats-in-a-story/](https://dannorth.net/whats-in-a-story/)

