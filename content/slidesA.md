!SLIDE subsection
# Configuraciones y Plugins

!SLIDE center
# Comencemos con Pruebas Unitarias

!SLIDE
# Configuraciones:

* Transacciones, Devise, Logger, Garbage Collector (_[Ir al artículo en
  el wiki](http://wiki.innku.com/articles/tips-para-mejorar-desempeno-en-suite-de-pruebas)_)
* Spork (_[Ir al artículo en el Wiki](http://wiki.innku.com/articles/configuracion-en-spork)_)

!SLIDE center
# Extra

* Guard (_[Ver artículo en el wiki](http://wiki.innku.com/articles/configuracion-en-guard)_)
* Jasmine

!SLIDE incremental
# Consejos en General:

* Let o Before? Let, Let!, lazy loading.
* Factories: Usen convenciones.
* Puedes agregar assertions en cuaquier parte.
* Corranlas en formato documentación.

!SLIDE

# Hablemos de Modelos

!SLIDE
# Usar shoulda para validaciones de Rails.

!SLIDE
# Timecop
    @@@ Ruby
    Timecop.freeze(Date.today + 30) do
      assert joe.mortgage_due?
    end

!SLIDE
# VCR
    @@@ Ruby
    require 'rubygems'
    require 'test/unit'
    require 'vcr'

    VCR.configure do |c|
      c.cassette_library_dir = 'fixtures/vcr_cassettes'
      c.hook_into :webmock # or :fakeweb
    end

    class VCRTest < Test::Unit::TestCase
      def test_example_dot_com
        VCR.use_cassette('synopsis') do
          response = Net::HTTP.get_response(URI('some_url'))
          assert_match /Example Domains/, response.body
        end
      end
    end



!SLIDE
# Tips para Controlador
* Son necesarias?

!SLIDE
# Tips para Vistas
* Probar condicionales en vistas

!SLIDE executable
# Tips para Mailers
    @@@ Ruby
    require 'spec_helper'
     
    describe User do
      it "sends a e-mail" do
        @user = User.make
        @user.send_instructions
        ActionMailer::Base.deliveries.last.to.should == [@user.email]
      end
    end

!SLIDE
# Tips para Observers

    @@@ Ruby
    describe Person, " when changing a name" do
      before(:each) do
        @person = Person.create! :name => "Pat Maddox"
      end

      # By default, don't run any observers
      it "should not register a name change" do
        lambda do 
          @person.update_attribute :name, "Don Juan Demarco" 
        end.should_not change(NameChange, :count)
      end

      # Run only a portion of code with certain observers turned on
      it "should register a name change with the observer turned on" do
        ActiveRecord::Observer.with_observers(:person_observer) do
          lambda do 
            @person.update_attribute :name, "Don Juan Demarco"
          end.should change(NameChange, :count).by(1)
        end
      end
    end


!SLIDE center
# No tengo tips para Presenters o Helpers... Todavia..

!SLIDE center
# Sigamos con Pruebas de Aceptación

!SLIDE center
# Cucumber o Request Spec. 

!SLIDE center
# No importa tanto...

!SLIDE center
# Si usas Cucumber bien

!SLIDE incremental
# Tips con Cucumber

* Enfoque declarativo o imperativo.
* Usar Capybara.
* Cambiar de driver de javascript.
* Database cleaner.
* No pruebes cosas que dependan de un objeto a bajo nivel.
