# Construindo uma Engine dentro da sua Gem

Atualmente estou trabalhando no time de Devtools na [RD Station](https://grnh.se/44d568232us) e nosso time é responsável por melhorar o fluxo de trabalho dos desenvolvedores.

Uma das tarefas recentes que nós entregamos foi de disponibilizar uma [gem](https://medium.com/rubybr/ruby-o-que-%C3%A9-uma-gem-d62aafc30724) que dentre algumas funcionalidades entregava uma [engine](https://guides.rubyonrails.org/engines.html) para facilitar a vida do pessoal. Bora ver como fazer isso?

Uma rails engine, para quem não sabe, nada mais é que uma "mini aplicação" rails, que permite que você adicione funcionalidades na aplicação principal (Rotas, serviços e outras coisas). Criar uma engine é bem simples:

    rails plugin new yoda-say --mountable

Apenas executando isso uma nova gem será gerada com todos os arquivos necessários para você sair usando. Eis que surge o primeiro problema:

Isso funciona bem para uma gem nova, não para uma que já existe (que era o meu caso).

![Bob Esponja sentado triste na mesa](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445674936/A-3EBIgFG.gif)

Para seguir com o exemplo, vamos criar uma gem (sem ser uma engine):

    rails plugin new yoda-say

ps: Nesse caso eu optei por criar especificamente um plugin rails para facilitar o exemplo

A diferença é que ela não tem o parâmetro **mountable** no final.

Agora vamos criar a classe que vai ser a nossa engine de fato e importar ela:


```ruby
# lib/yoda/say/engine.rb

module Yoda
  module Say
    class Engine < ::Rails::Engine
    end
  end
end

# lib/yoda/say.rb
require "yoda/say/engine"
``` 


Isso é suficiente para nossa engine estar funcionando. O unico problema é que ela não faz nada. ![Homem esfregando as temporas](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445676922/mZbeNg6gG.gif)

Agora vamos adicionar algumas coisas necessárias para nossa engine fazer alguma coisa:

```ruby
# config/routes.rb
Yoda::Say::Engine.routes.draw do
  get '/', to: 'home#index'
end

# app/controllers/home_controller.rb
class HomeController < ActionController::Base
  def index
    render json: { yoda_say: '' }
  end
end
``` 


Pronto, agora sim. Se você quiser testar essa etapa, pode colocar na sua app rails dentro do arquivo de rotas:

    Rails.application.routes.draw do
      mount Yoda::Say::Engine => '/yoda'
    end

Mas como a gente adiciona testes? ![Mulher dando um sorriso e ficando chocada em seguida](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445678705/GYdRQHPIH.gif)

Acontece que quando nós geramos a nossa gem (como plugin rails) foi criada uma aplicação boba para testarmos (ela está em `test/dummy`), então só precisamos mudar algumas coisas nela:


```ruby
# test/dummy/config/routes.rb
Rails.application.routes.draw do
  mount Yoda::Say::Engine => '/yoda'
end

# test/controllers/home_controller_test.rb
require 'test_helper'

class HomeControllerTest < ActiveSupport::TestCase
  include Rack::Test::Methods

  def app
    Rails.application
  end

  test 'succeeds' do
    get '/yoda'
    assert last_response.ok?
    assert last_response.body == '{"yoda_say":""}'
  end
end
``` 

Se você executar `rake test` vai estar tudo funcionando. ![barney stinson fazendo sinal de joia dentro de um carro](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445680275/t-gpTHVd5.gif)

O Plot Twist
------------

Você não precisa de uma Engine.

![Joe Cruz se virando com um olhar de estranhamento](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445682168/YZZ50VcpP.gif)

Não, você não leu errado.

O que acontece é simples, apesar da Rails Engine ser uma coisa muito legal, ela carrega o "peso do Rails" junto dela (o que em alguns casos é o que nós queremos, então tudo bem).

Além disso ela só pode ser plugada em uma aplicação Rails, o que pode ser um problema caso você queira oferecer isso para alguém que usa [Sinatra](http://sinatrarb.com/) por exemplo.

A alternativa é muito simples, nós vamos usar uma [Rack App](https://github.com/rack/rack)! Bora ver como isso fica.

Primeiro vamos criar a nossa App:


```
# lib/yoda/say/app.rb
module Yoda
  module Say
    class App

      def self.call(env)
        request = Rack::Request.new(env)
        [status, headers, body]
      end

    end
  end
end
``` 


A aplicação é bem simples, você recebe um request, processa ele (nós vamos fazer isso no controller) e devolve um array com três posições: status (número), header(hash) e body (array).

Vamos ajustar nossa aplicação para ela funcionar corretamente:


```
# lib/yoda/say/app.rb

require_relative 'home_controller'

module Yoda
  module Say
    class App

      def self.call(env)
        request = Rack::Request.new(env)
        ::HomeController.index(request)
      end

    end
  end
end

# lib/yoda/say/home_controller.rb
class HomeController
  def self.index(_request)
    status = 200
    headers = {}
    body = [{ yoda_say: '' }.to_json]
    [status, headers, body]
  end
end
``` 


Pronto, nossa aplicação está funcional agora. Antes de rodar o teste, precisamos alterar a rota da nossa app dummy em `test/dummy/config/routes.rb` para `mount Yoda::Say::App => '/yoda'`.

Dito isso, podemos deletar o arquivo da engine e as definições de rotas que já não são mais necessárias.

Pra fechar
----------

Nesse tutorial você viu **duas** maneiras de entregar recursos avançados dentro da sua aplicação principal através de uma gem. A primeira parte foi com uma Rails Engine e na sequência mudamos tudo para usar uma Rack App.

Você pode estar se perguntando, como eu escolho qual usar?

Tem muito a ver com a sua necessidade, usar o Rack é bem simples além de permitir integrar facilmente com vários frameworks, mas quando seu caso de uso é mais complexo você pode acabar tendo que desenvolver muitas coisas que já existem numa aplicação Rails. Por exemplo, se você precisar de duas ou três rotas, cache e alguns outros detalhes, vale muito mais a pena você já começar usando uma engine.

Você pode consultar o código fonte no [meu Github](https://github.com/angeliski/yoda-say) Qualquer coisa me manda uma mensagem.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski

Referências
-----------
- https://guides.rubyonrails.org/engines.html
- https://github.com/rack/rack
- https://medium.com/rubybr/ruby-o-que-%C3%A9-uma-gem-d62aafc30724