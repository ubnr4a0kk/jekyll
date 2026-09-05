# frozen_string_literal: true

module Jekyll
  # NullReader is a stub reader used when reading site content is disabled,
  # such as during certain command line operations or dry runs.
  class NullReader < Reader
    # Read nothing and return an empty array of files.
    #
    # @return [Array] Empty array representing no read files.
    def read
      []
    end
  end
end