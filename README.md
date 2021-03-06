##Gem modifications

This is a fork from the original Country State Select to use the countries gem and add new methods:

  1. cs_update - This methods updates the list of cities of the city-state gem. It retrieves the list from MaxMind and updates it.
  2. countries_collection_locale(locale_code='en') - This method is similar to the method countries_collection, but it uses a locale to get country info from countries gem instead of city-state gem.    
  3. countries_except_locale(locale_code='en',*except) - This method is similar to the method countries_except(*except), but it uses countries gem instead of city-state gem. 
  4. country - This method provides access to the underlying countries' Country object.
  5. cities_state - This method provides access to the underlying city-state's CS object.

##Country State Select [![Build Status](https://travis-ci.org/arvindvyas/Country-State-Select.svg?branch=master)](https://travis-ci.org/arvindvyas/Country-State-Select)  [![Code Climate](https://codeclimate.com/github/arvindvyas/Country-State-Select/badges/gpa.svg)](https://codeclimate.com/github/arvindvyas/Country-State-Select)

Country State Select is a library that provides an easy API to generate Country , State / Province  and City dropdowns for use in forms.

When implemented correctly, a State / Province dropdown is filled with appropriate regions based upon what Country a user has selected .

For instance, if a user chooses "United States of America" for a Country dropdown, the State dropdown will be filled with the 50 appropriate states plus the District of Columbia also then user can list city according to state selection but currently cities are limited.

The data for Countries and States is populated by the excellent [city-state](https://github.com/loureirorg/city-state) gem. 

#Demo Of the Gem
  https://country-state-select.herokuapp.com

#Getting Started

Country State Select is released as a Ruby Gem and requires the Rails environment as well.

To install, simply add the following to your Gemfile.

    gem 'country_state_select'
  
Then run 
    
    bundle install


Don't forget to restart your server after you install it.


## chosen JavaScript assets
  The application makes use of the chosen-rails library for a better user experience.  This can be disabled if desired.  
  
  If you're using chosen-rails, please add the following to app/assets/javascripts/application.js:
  
      //= require chosen-jquery


## chosen CSS assets

  You'll also need to add the chosen-rails stylesheet.

  If you're using chosen-rails, please add the following to app/assets/stylesheets/application.css:

    *= require chosen

 or
    
    @import "chosen";
    
    
# Form Implementation in SimpleForm

Implementation in a view is simple. 

Just create a simple_form object like so:

    <%= simple_form_for @form_object do |f| 
      f.button :submit
    end %>
    
Once your form object is created, add in fields for your user to select a Country and State / Province.      

## Country Field

To add the Country field, add an input field like the following:

    <%= f.input :country_field, collection: CountryStateSelect.countries_collection %>
  
The above code is simply a normal simple_form field object with a collection of countries passed to it using the CountryStateSelect library.  

You can pass any of the normal options that a field object accepts - :label, :input_html, etc.    
    
## States / Provinces Field

To list the States / Provinces, add an input field like the following:

    <%= options = { form: f, field_names: { :country => :country_field, :state => :state_field } }
    
    f.input :state_field, CountryStateSelect.state_options(options) %>
    
It's important to note that the :country and :state keys in the :field_names object must contain the correct values in order for the correct States / Provinces to load into the window. 

Other than that, it's yet another standard simple_form field object.  

Any options that are valid in simple_form field can be passed into the options hash.

# Required JavaScript

The magic of the library is the JavaScript that is performed on the fields.  This JavaScript needs to be included in order for the proper State / Provinces to be selected based upon your Country selection.  

Add this JavaScript to the page that is loading your form:

    $(document).on('ready page:load', function() {
      return CountryStateSelect({
        country_id: "country_field_id",
        state_id: "state_field_id"
      });
    });

If you are using CoffeeScript, use the following:

    $(document).on 'ready page:load', ->
        CountryStateSelect({ country_id: "country_field_id", state_id: "state_field_id" })

The country_id accepts the form ID for the country field.  The state_id accepts the form ID for the state field.

The easiest way to determine the ID is to view the source in the web browser. (Or, you could set it in the options hash.)

The ID being referred to is like the following in a select menu:

    <select id="country_field_id"></select>
   
# chosen UI improvements (optional)

The CountryStateSelect interface offers the option to turn the Chosen library on or off.  

To turn it on, set it to true:

    $(document).on('ready page:load', function() {
      return CountryStateSelect({
        chosen_ui: true,
        ...
        ...
        ...
      });
    }); 

To turn it off, set it to false:

    $(document).on('ready page:load', function() {
      return CountryStateSelect({
        chosen_ui: false,
        ...
        ...
        ...
      });
    });
    

##Contributing to Country State Select

  Fork, fix, then send a pull request.
 
