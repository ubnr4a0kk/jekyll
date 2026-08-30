# frozen_string_literal: true

module Jekyll
  class Generator
    attr_reader :config

    # Initialize a new Generator instance.
    #
    # config - The Hash configuration.
    def initialize(config = {})
      @config = config
    end

    # Set or get the priority level for this Generator subclass.
    #
    # priority_sym - Optional Symbol representing the priority level.
    #
    # Returns the priority Symbol.
    def self.priority(priority_sym = nil)
      if priority_sym.nil?
        @priority ||= :normal
      else
        @priority = priority_sym
      end
    end

    def self.priorities
      {
        :lowest  => -100,
        :low     => -50,
        :normal  => 0,
        :high    => 50,
        :highest => 100,
      }
    end

    def self.<=>(other)
      priorities[priority] <=> priorities[other.priority]
    end

    def priority
      self.class.priority
    end

    def <=>(other)
      self.class <=> other.class
    end

    # Subclasses must implement a #generate method that receives the Site object.
    #
    # site - The Jekyll::Site instance to generate content for.
    #
    # Returns nothing.
    def generate(site)
      raise NotImplementedError, "Subclasses of Jekyll::Generator must implement #generate"
    end
  end
end