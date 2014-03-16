# Páginas de Erro 404 e 500

Para criar paginas de erro 404 e 500 vamos primeiro adicionar o codigo no ```app/controllers/application_controller.rb```:

	class ApplicationController < ActionController::Base
	  # ...
	
	  unless Rails.application.config.consider_all_requests_local
	    rescue_from Exception, with: lambda { |exception| render_error 500, exception }
	    rescue_from ActionController::RoutingError, ActionController::UnknownController, ::AbstractController::ActionNotFound, ActiveRecord::RecordNotFound, with: lambda { |exception| render_error 404, exception }
	  end
	
	  private
	  def render_error(status, exception)
	    respond_to do |format|
	      format.html { render template: "errors/error_#{status}", layout: 'layouts/application', status: status }
	      format.all { render nothing: true, status: status }
	    end
	  end
	
	  # ...
	end

Depois é necessario criar um controlador para mostrar essas páginas. Crie o controlador  ```app/controllers/errors_controller.rb```:

	class ErrorsController < ApplicationController
	  def error_404
	    @not_found_path = params[:not_found]
	  end
	
	  def error_500
	  end
	end

Criei os templates para as duas páginas de erro em ```app/views/errors/```.

Por ultimo, adicione no fim do arquivo ```config/routes.rb``` uma rota para o erro 404:

	unless Rails.application.config.consider_all_requests_local
	    match '*not_found', to: 'errors#error_404'
	end

Essas págians de erro vão apenas ser mostradas no ambiente de produção. Para testar no ambiente de desenvolvimento, retire o seguinte codigo do ```application_controller.rb``` e do ```config/routes.rb```:

	Rails.application.config.consider_all_requests_local

### Link
* [Rambling Labs - Rails 3.1 - Adding custom 404 and 500 error pages](http://ramblinglabs.com/blog/2012/01/rails-3-1-adding-custom-404-and-500-error-pages)

---
