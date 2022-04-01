# hello-world
const Browser = require('zombie');

// В тесте мы будем отправлять запросы на адрес http://example.com/signup
// однако эти запросы будут обслуживаться установленным локально 
// сервером на порту 3000 с адресом localhost:3000
Browser.localhost('example.com', 3000);

// describe описывает сценарий теста, 
// причем блоки describe могут вкладываться друг в друга
describe('Пользователь заходит на страницу регистрации', function() {

  // создаем объект, имитирующий браузер
  const browser = new Browser();
  
  // Действие, выполняемое до теста - переход на нужную страницу
  before(function(done) {
    browser.visit('/signup', done);
  });

  describe('пользователь заполняет и отправляет форму', function() {

    before(function(done) {
      browser
        .fill('email',    'tester@example.com')
        .fill('password', '123456')
        .fill('password-confirm', '123456')
        .pressButton('Зарегистрироваться', done);
    });

    // it описывает конкретное требование и код для его проверки
    it('регистрация должна быть успешной', function() {
      browser.assert.success();
    });

    it('пользователь должен увидеть приветственное сообщение', function() {
      // Уведомление должно вывестись в элементе с классом notification 
      // (используется CSS-синтаксис селекторов)
      browser.assert.text('.notification', 'Вы успешно зарегистрированы');
    });
  });
});
