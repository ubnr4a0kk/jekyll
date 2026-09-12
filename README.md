# frozen_string_literal: true

module Jekyll
  # Hooks provide container objects for callback registration in the lifecycle
  # of Jekyll site rendering and document processing.
  module Hooks
    DEFAULT_PRIORITY = 20

    # Specific priorities for ordering hook execution.
    PRIORITY_MAP = {
      :lowest  => 0,
      :low     => 10,
      :normal  => 20,
      :high    => 30,
      :highest => 40,
    }.freeze

    @registry = {}

    class << self
      # Register a new hook callback for a given container owner and event name.
      #
      # owner    - Symbol representing the target entity (e.g. :site, :pages, :posts, :documents)
      # event    - Symbol representing the event name (e.g. :post_render, :post_write)
      # priority - Priority integer or symbol (:lowest, :low, :normal, :high, :highest)
      # block    - Proc to be executed when event is triggered
      def register(owner, event, priority: DEFAULT_PRIORITY, &block)
        priority = PRIORITY_MAP[priority] if priority.is_a?(Symbol)

        @registry[owner] ||= {}
        @registry[owner][event] ||= []
        @registry[owner][event] << [priority, block]
        @registry[owner][event].sort_by! { |p, _| -p }
      end

      # Trigger callbacks registered for a container owner and event.
      def trigger(owner, event, *args)
        return unless @registry.dig(owner, event)

        @registry[owner][event].each do |_, block|
          block.call(*args)
        end
      end
    end
  end
end